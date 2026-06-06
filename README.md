# 🗂️ CV Management Database System

> A structured relational database solution for digitizing and streamlining recruitment workflows — built as an academic project at **Tribhuvan University, Pulchowk Campus**.

---

## 👥 Team

| Name | Roll No. |
|------|----------|
| Darpan Giri | 080BCT024 |
| Kushal Gautam | 080BCT040 |
| Lavraj Karn | 080BCT043 |
| Mahesh Bhandari | 080BCT045 |

**Department:** Electronics and Computer Engineering

---

## 📌 Overview

Traditional CV management — storing Word documents and PDFs in nested folders — creates data silos, redundancy, and painfully slow candidate searches. This project replaces that with a fully normalized **MySQL relational database** that lets HR teams filter candidates by skill, experience, education, or certification in seconds.

---

## 🎯 Objectives

- **Centralized storage** for all candidate profiles and professional histories
- **Data security** by restricting access to sensitive contact information
- **Efficient retrieval** using optimized SQL queries for recruitment filtering
- **Scalable design** supporting easy addition of skills, experiences, and certifications
- **Zero redundancy** through normalization up to 3NF

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| **MySQL 8.0+** | Core relational database engine |
| **XAMPP v3.3.0** | Local development environment |
| **phpMyAdmin 5.x** | GUI for database management |
| **SQL (DDL + DML)** | Schema creation and data manipulation |

---

## 🧩 Database Schema

The system comprises **9 normalized tables**:

```
User
├── User_Phone          (multivalued: multiple phone numbers)
├── Education           (1:N — one user, many degrees)
├── Experience          (1:N — one user, many jobs)
├── Project             (1:N — one user, many projects)
│   └── Project_Technology  (multivalued: multiple tech stacks per project)
├── Certificate         (1:N — one user, many certifications)
└── User_Skill          (M:N junction — users ↔ skills + proficiency level)
    └── Skill
```

### Key Entities & Attributes

| Entity | Primary Key | Notable Attributes |
|--------|------------|-------------------|
| User | `user_id` | full_name, email, address, gender, linkedin_url, date_of_birth |
| Education | `education_id` | degree, major, institution, gpa, start_date, end_date |
| Experience | `experience_id` | job_title, company_name, location, job_description |
| Skill | `skill_id` | skill_name, skill_type |
| Project | `project_id` | project_name, description, project_link |
| Certificate | `certification_id` | certification_name, issuing_organization, issue_date, expiry_date |

---

## 🔗 Entity Relationships

| Relationship | Cardinality | Implementation |
|---|---|---|
| User → Education | One-to-Many | FK `user_id` in Education |
| User → Experience | One-to-Many | FK `user_id` in Experience |
| User → Project | One-to-Many | FK `user_id` in Project |
| User → Certificate | One-to-Many | FK `user_id` in Certificate |
| User ↔ Skill | Many-to-Many | Junction table `User_Skill` |

All FK constraints use `ON DELETE CASCADE` to maintain referential integrity.

---

## 📐 Normalization (up to 3NF)

| Normal Form | Requirement | How We Satisfy It |
|---|---|---|
| **1NF** | Atomic values only | Multivalued attributes (`phone`, `technologies_used`) moved to separate tables |
| **2NF** | No partial dependencies | All non-key attributes in junction tables depend on the full composite key |
| **3NF** | No transitive dependencies | Every non-key attribute depends only on the primary key, not on other non-key columns |

---

## 🚀 Getting Started

### Prerequisites
- [XAMPP](https://www.apachefriends.org/) (includes MySQL + Apache + phpMyAdmin)

### Setup

```bash
# 1. Start Apache and MySQL from XAMPP Control Panel

# 2. Open phpMyAdmin
http://localhost/phpmyadmin

# 3. Create the database
CREATE DATABASE CV_Management_System;

# 4. Run all DDL scripts (table creation)
# 5. Run all DML scripts (sample data insertion)
```

### Running Queries

```sql
-- Find all candidates with Python skills at Advanced level
SELECT u.full_name, s.skill_name, us.proficiency_level
FROM User u
JOIN User_Skill us ON u.user_id = us.user_id
JOIN Skill s ON us.skill_id = s.skill_id
WHERE s.skill_name = 'Python' AND us.proficiency_level = 'Advanced';

-- List candidates with their latest experience
SELECT u.full_name, e.job_title, e.company_name, e.end_date
FROM User u
JOIN Experience e ON u.user_id = e.user_id
ORDER BY e.end_date DESC;

-- Find candidates by degree
SELECT u.full_name, ed.degree, ed.major, ed.institution, ed.gpa
FROM User u
JOIN Education ed ON u.user_id = ed.user_id
WHERE ed.degree = 'Bachelor of Engineering';
```

---

## ☁️ Deployment Options

| Environment | How |
|---|---|
| **Local** | XAMPP as described above |
| **Server** | Install MySQL on Ubuntu, manage via MySQL Workbench |
| **Cloud** | Migrate to AWS RDS / Google Cloud SQL / Azure MySQL using `mysqldump` |

---

## 🔮 Future Improvements

- [ ] **Web Interface** — Build a front-end with PHP, Django, or Node.js
- [ ] **Role-Based Access Control (RBAC)** — Protect sensitive candidate data
- [ ] **Full-Text Search** — Index `job_description`, `skill_name`, and `project_description`
- [ ] **REST API** — Enable integration with external ATS and job portals
- [ ] **Automated Backups** — Prevent data loss with scheduled backup procedures
- [ ] **Cloud Migration** — Deploy on AWS RDS or Google Cloud SQL

---

## 📚 References

- Ramakrishnan, R. & Gehrke, J. — *Database Management Systems* — McGraw Hill
- Silberschatz, A., Korth, H.F. & Sudarshan, S. — *Database System Concepts* — McGraw Hill
- Elmasri, R. & Navathe, S.B. — *Fundamentals of Database Systems*, 5th Ed. — Pearson (2008)
- [MySQL Official Documentation](https://dev.mysql.com/doc/)
- [W3Schools SQL Tutorial](https://www.w3schools.com/sql/)

---

## 📄 License

This project was developed for academic purposes at Tribhuvan University, Pulchowk Campus.
