# NIVARAN – Database Documentation

## 1. Project Overview

**Project Name:** NIVARAN – AI-Assisted Grievance Redressal System

**Organization:** Chhatrapati Shahu Ji Maharaj University (CSJMU), Kanpur, Uttar Pradesh

NIVARAN is a digital grievance redressal system designed to allow students/users to submit grievances online and track their status throughout the complete resolution process.

The database is responsible for storing and managing:

* User information
* Different user roles
* Login and authentication information
* Grievance details
* Grievance categories
* Grievance status
* Grievance assignment
* Grievance forwarding
* Comments and remarks
* Notifications
* Grievance history
* Profile information
* Resolution details
* Administrative information

The database acts as the central storage layer between the frontend and backend of the NIVARAN system.

---

# 2. Objectives of the Database

The main objectives of the NIVARAN database are:

1. To securely store user information.
2. To maintain authentication and authorization data.
3. To store all submitted grievances.
4. To maintain the complete lifecycle of every grievance.
5. To assign grievances to the appropriate authority.
6. To store grievance status updates.
7. To maintain grievance forwarding history.
8. To store remarks and comments given by authorities.
9. To generate notifications for users and authorities.
10. To maintain an audit/history of grievance activities.
11. To avoid duplicate and inconsistent data.
12. To provide fast and reliable data retrieval.
13. To maintain data integrity and security.
14. To support future AI-based grievance classification and prioritization.

---

# 3. Database Architecture

The NIVARAN database follows a structured relational database architecture.

The overall application architecture can be represented as:

Frontend
|
| HTTP/HTTPS Request
↓
Backend / API Layer
|
| SQL Queries / ORM
↓
Database
|
├── Users
├── Roles
├── Grievances
├── Categories
├── Assignments
├── Comments
├── Notifications
└── Grievance History

The frontend does not directly communicate with the database.

The correct communication flow is:

User
↓
Frontend
↓
Backend API
↓
Database
↓
Backend API
↓
Frontend
↓
User

This provides better security, validation, authentication, and access control.

---

# 4. Recommended Database

The recommended database for NIVARAN is:

**PostgreSQL / MySQL**

Either relational database can be used for the project.

The database should support:

* Primary Keys
* Foreign Keys
* Unique Constraints
* NOT NULL Constraints
* CHECK Constraints
* Indexing
* Transactions
* Referential Integrity
* Timestamps
* Role-based access
* Secure password storage

---

# 5. Database Naming Convention

The following naming conventions should be followed:

* Table names should use `snake_case`.
* Column names should use `snake_case`.
* Primary key should generally be named `id`.
* Foreign keys should follow `<table_name>_id`.
* Boolean columns should use meaningful names such as `is_active`.
* Timestamp columns should use names such as `created_at` and `updated_at`.

Example:

```text
users
grievances
grievance_categories
grievance_history
notifications
```

---

# 6. Main Database Entities

The major entities of NIVARAN are:

1. Users
2. Roles
3. Grievance Categories
4. Grievances
5. Grievance Assignments
6. Grievance Comments
7. Grievance History
8. Notifications
9. Attachments
10. AI Analysis

---

# 7. Roles

NIVARAN uses role-based access control.

The major roles are:

| Role           | Description                                    |
| -------------- | ---------------------------------------------- |
| User           | Submits and tracks grievances                  |
| Manager        | Handles and manages grievances                 |
| Associate Dean | Reviews and processes grievances               |
| Assistant Dean | Reviews and processes grievances               |
| Dean R&D       | Higher-level authority and escalation handling |

Roles should be stored separately instead of repeatedly storing role names in multiple tables.

---

# 8. Users Table

The `users` table stores information about all registered users and authorities.

### Table: users

| Column        | Data Type    | Constraint       | Description               |
| ------------- | ------------ | ---------------- | ------------------------- |
| id            | BIGINT       | PRIMARY KEY      | Unique user ID            |
| name          | VARCHAR(100) | NOT NULL         | Full name                 |
| email         | VARCHAR(150) | UNIQUE, NOT NULL | User email                |
| password_hash | VARCHAR(255) | NOT NULL         | Encrypted/hashed password |
| role_id       | BIGINT       | FOREIGN KEY      | User role                 |
| phone         | VARCHAR(20)  | NULL             | Contact number            |
| department    | VARCHAR(150) | NULL             | Department                |
| designation   | VARCHAR(150) | NULL             | Designation               |
| is_active     | BOOLEAN      | DEFAULT TRUE     | Account status            |
| created_at    | TIMESTAMP    | NOT NULL         | Account creation time     |
| updated_at    | TIMESTAMP    | NOT NULL         | Last update time          |

### Purpose

This table is used for:

* Registration
* Login
* Authentication
* Authorization
* Profile management
* Role-based access control

---

# 9. Roles Table

### Table: roles

| Column      | Data Type   | Constraint       | Description      |
| ----------- | ----------- | ---------------- | ---------------- |
| id          | BIGINT      | PRIMARY KEY      | Unique role ID   |
| role_name   | VARCHAR(50) | UNIQUE, NOT NULL | Name of role     |
| description | TEXT        | NULL             | Role description |

Example roles:

```text
User
Manager
Associate Dean
Assistant Dean
Dean R&D
```

---

# 10. Grievance Categories Table

This table stores predefined categories of grievances.

### Table: grievance_categories

| Column        | Data Type    | Constraint       | Description          |
| ------------- | ------------ | ---------------- | -------------------- |
| id            | BIGINT       | PRIMARY KEY      | Category ID          |
| category_name | VARCHAR(100) | UNIQUE, NOT NULL | Category name        |
| description   | TEXT         | NULL             | Category description |
| is_active     | BOOLEAN      | DEFAULT TRUE     | Category status      |
| created_at    | TIMESTAMP    | NOT NULL         | Creation time        |

Possible categories:

```text
Academic
Examination
Hostel
Fee
Scholarship
Infrastructure
Administrative
Technical
Library
Transport
Other
```

---

# 11. Grievances Table

The `grievances` table is the core table of the NIVARAN database.

It stores every grievance submitted by a user.

### Table: grievances

| Column              | Data Type    | Constraint            | Description                    |
| ------------------- | ------------ | --------------------- | ------------------------------ |
| id                  | BIGINT       | PRIMARY KEY           | Unique grievance ID            |
| grievance_number    | VARCHAR(30)  | UNIQUE, NOT NULL      | Public grievance reference     |
| user_id             | BIGINT       | FOREIGN KEY, NOT NULL | User who submitted grievance   |
| category_id         | BIGINT       | FOREIGN KEY           | Grievance category             |
| subject             | VARCHAR(255) | NOT NULL              | Grievance subject              |
| description         | TEXT         | NOT NULL              | Complete grievance description |
| priority            | VARCHAR(20)  | DEFAULT 'Normal'      | Priority                       |
| status              | VARCHAR(30)  | DEFAULT 'Submitted'   | Current status                 |
| current_assignee_id | BIGINT       | FOREIGN KEY           | Current authority              |
| submitted_at        | TIMESTAMP    | NOT NULL              | Submission time                |
| updated_at          | TIMESTAMP    | NOT NULL              | Last update                    |
| resolved_at         | TIMESTAMP    | NULL                  | Resolution time                |
| closed_at           | TIMESTAMP    | NULL                  | Closing time                   |

---

# 12. Grievance Status

A grievance moves through different stages.

Recommended statuses:

```text
Submitted
Under Review
Assigned
In Progress
Forwarded
Resolved
Rejected
Closed
Reopened
```

Example workflow:

```text
Submitted
    ↓
Under Review
    ↓
Assigned
    ↓
In Progress
    ↓
Forwarded
    ↓
Resolved
    ↓
Closed
```

If the user is not satisfied:

```text
Resolved
    ↓
Reopened
    ↓
Under Review
```

---

# 13. Grievance Priority

The priority of a grievance can be categorized as:

```text
Low
Normal
High
Urgent
```

Priority can initially be assigned manually.

In future versions, AI can automatically calculate priority based on grievance content.

---

# 14. Grievance Assignment Table

This table records which authority is assigned to a grievance.

### Table: grievance_assignments

| Column        | Data Type | Constraint  | Description              |
| ------------- | --------- | ----------- | ------------------------ |
| id            | BIGINT    | PRIMARY KEY | Assignment ID            |
| grievance_id  | BIGINT    | FOREIGN KEY | Grievance                |
| assigned_to   | BIGINT    | FOREIGN KEY | Authority                |
| assigned_by   | BIGINT    | FOREIGN KEY | Person making assignment |
| assigned_at   | TIMESTAMP | NOT NULL    | Assignment time          |
| unassigned_at | TIMESTAMP | NULL        | End of assignment        |
| remarks       | TEXT      | NULL        | Assignment remarks       |

This table is important because a grievance may be assigned to different authorities during its lifecycle.

---

# 15. Grievance Comments Table

Authorities may add remarks or comments to a grievance.

### Table: grievance_comments

| Column       | Data Type | Constraint  | Description           |
| ------------ | --------- | ----------- | --------------------- |
| id           | BIGINT    | PRIMARY KEY | Comment ID            |
| grievance_id | BIGINT    | FOREIGN KEY | Related grievance     |
| user_id      | BIGINT    | FOREIGN KEY | Person adding comment |
| comment      | TEXT      | NOT NULL    | Comment/remark        |
| created_at   | TIMESTAMP | NOT NULL    | Comment time          |

Comments may include:

* Additional information requests
* Investigation remarks
* Department response
* Resolution explanation
* Administrative remarks

---

# 16. Grievance History Table

The history table maintains the complete lifecycle of a grievance.

### Table: grievance_history

| Column       | Data Type    | Constraint  | Description            |
| ------------ | ------------ | ----------- | ---------------------- |
| id           | BIGINT       | PRIMARY KEY | History ID             |
| grievance_id | BIGINT       | FOREIGN KEY | Related grievance      |
| action_by    | BIGINT       | FOREIGN KEY | User performing action |
| action       | VARCHAR(100) | NOT NULL    | Action performed       |
| old_status   | VARCHAR(30)  | NULL        | Previous status        |
| new_status   | VARCHAR(30)  | NULL        | New status             |
| remarks      | TEXT         | NULL        | Additional information |
| created_at   | TIMESTAMP    | NOT NULL    | Action timestamp       |

Example:

```text
Grievance Submitted
        ↓
Assigned to Manager
        ↓
Status changed to Under Review
        ↓
Forwarded to Associate Dean
        ↓
Status changed to In Progress
        ↓
Resolved
        ↓
Closed
```

Every important action should be recorded in the history table.

---

# 17. Notifications Table

The notification system informs users about important grievance updates.

### Table: notifications

| Column            | Data Type    | Constraint    | Description           |
| ----------------- | ------------ | ------------- | --------------------- |
| id                | BIGINT       | PRIMARY KEY   | Notification ID       |
| user_id           | BIGINT       | FOREIGN KEY   | Notification receiver |
| grievance_id      | BIGINT       | FOREIGN KEY   | Related grievance     |
| title             | VARCHAR(255) | NOT NULL      | Notification title    |
| message           | TEXT         | NOT NULL      | Notification message  |
| notification_type | VARCHAR(50)  | NULL          | Notification type     |
| is_read           | BOOLEAN      | DEFAULT FALSE | Read/unread status    |
| created_at        | TIMESTAMP    | NOT NULL      | Notification time     |

Examples:

```text
Grievance submitted successfully.
Your grievance has been assigned.
Your grievance is under review.
Additional information is required.
Your grievance has been forwarded.
Your grievance has been resolved.
Your grievance has been closed.
```

---

# 18. Attachments Table

Users may upload supporting documents with their grievances.

### Table: attachments

| Column       | Data Type    | Constraint  | Description        |
| ------------ | ------------ | ----------- | ------------------ |
| id           | BIGINT       | PRIMARY KEY | Attachment ID      |
| grievance_id | BIGINT       | FOREIGN KEY | Related grievance  |
| uploaded_by  | BIGINT       | FOREIGN KEY | Uploading user     |
| file_name    | VARCHAR(255) | NOT NULL    | Original file name |
| file_path    | TEXT         | NOT NULL    | Storage location   |
| file_type    | VARCHAR(100) | NULL        | File type          |
| file_size    | BIGINT       | NULL        | File size          |
| uploaded_at  | TIMESTAMP    | NOT NULL    | Upload time        |

Files should not generally be stored directly inside the relational database.

Instead:

```text
Database → stores file metadata/path
File Storage → stores actual document
```

---

# 19. AI Analysis Table

NIVARAN can later use Artificial Intelligence to assist with grievance classification and prioritization.

### Table: ai_analysis

| Column             | Data Type    | Constraint  | Description           |
| ------------------ | ------------ | ----------- | --------------------- |
| id                 | BIGINT       | PRIMARY KEY | Analysis ID           |
| grievance_id       | BIGINT       | FOREIGN KEY | Related grievance     |
| predicted_category | VARCHAR(100) | NULL        | AI predicted category |
| predicted_priority | VARCHAR(30)  | NULL        | AI predicted priority |
| confidence_score   | DECIMAL(5,4) | NULL        | AI confidence         |
| sentiment          | VARCHAR(30)  | NULL        | Detected sentiment    |
| model_version      | VARCHAR(50)  | NULL        | AI model version      |
| analyzed_at        | TIMESTAMP    | NOT NULL    | Analysis time         |

AI should assist the authorities rather than automatically making irreversible decisions.

---

# 20. Entity Relationship Overview

The major relationships are:

```text
ROLES
  |
  | 1
  |
  | M
USERS
  |
  | 1
  |
  | M
GRIEVANCES
  |
  ├────────── M:1 ────────── GRIEVANCE_CATEGORIES
  |
  ├────────── 1:M ────────── GRIEVANCE_ASSIGNMENTS
  |
  ├────────── 1:M ────────── GRIEVANCE_COMMENTS
  |
  ├────────── 1:M ────────── GRIEVANCE_HISTORY
  |
  ├────────── 1:M ────────── NOTIFICATIONS
  |
  ├────────── 1:M ────────── ATTACHMENTS
  |
  └────────── 1:M ────────── AI_ANALYSIS
```

---

# 21. Relationship Details

## 21.1 Role → Users

One role can belong to many users.

Relationship:

```text
roles 1 ─────── M users
```

Example:

```text
User Role
   ↓
Many Students
```

---

## 21.2 User → Grievances

One user can submit multiple grievances.

Relationship:

```text
users 1 ─────── M grievances
```

---

## 21.3 Category → Grievances

One category can contain many grievances.

Relationship:

```text
grievance_categories 1 ─────── M grievances
```

---

## 21.4 Grievance → Comments

One grievance can have multiple comments.

Relationship:

```text
grievances 1 ─────── M grievance_comments
```

---

## 21.5 Grievance → History

One grievance can have multiple history records.

Relationship:

```text
grievances 1 ─────── M grievance_history
```

---

## 21.6 Grievance → Assignments

One grievance can have multiple assignments over its lifecycle.

Relationship:

```text
grievances 1 ─────── M grievance_assignments
```

---

## 21.7 Grievance → Notifications

A grievance may generate multiple notifications.

Relationship:

```text
grievances 1 ─────── M notifications
```

---

# 22. Primary Keys

Every major table should have a unique primary key.

Examples:

```text
users.id
roles.id
grievances.id
grievance_categories.id
grievance_assignments.id
grievance_comments.id
grievance_history.id
notifications.id
attachments.id
ai_analysis.id
```

Primary keys uniquely identify each record.

---

# 23. Foreign Keys

Foreign keys establish relationships between tables.

Examples:

```text
users.role_id
        ↓
roles.id
```

```text
grievances.user_id
        ↓
users.id
```

```text
grievances.category_id
        ↓
grievance_categories.id
```

```text
grievance_comments.grievance_id
        ↓
grievances.id
```

Foreign keys maintain referential integrity.

---

# 24. Database Constraints

The following constraints should be used.

## PRIMARY KEY

Ensures every record has a unique identifier.

## FOREIGN KEY

Maintains relationships between tables.

## NOT NULL

Ensures required information cannot be empty.

## UNIQUE

Prevents duplicate values.

Example:

```text
users.email
grievances.grievance_number
roles.role_name
```

## DEFAULT

Provides a default value.

Example:

```text
is_active = TRUE
is_read = FALSE
status = 'Submitted'
priority = 'Normal'
```

## CHECK

Can be used to restrict values.

Example:

```text
priority IN ('Low', 'Normal', 'High', 'Urgent')
```

---

# 25. Normalization

The NIVARAN database should follow database normalization principles.

The main objective is to reduce:

* Data redundancy
* Data duplication
* Update anomalies
* Insert anomalies
* Delete anomalies

The database should preferably be normalized up to **Third Normal Form (3NF)** where practical.

For example, role information should not be repeatedly stored in the users table as complete role descriptions.

Instead:

```text
users.role_id
       ↓
roles.id
```

Similarly, category information should be maintained separately.

```text
grievances.category_id
       ↓
grievance_categories.id
```

---

# 26. Indexing

Indexes should be created on frequently searched columns.

Recommended indexes include:

```text
users.email
users.role_id
grievances.user_id
grievances.status
grievances.category_id
grievances.current_assignee_id
grievances.grievance_number
grievances.submitted_at
notifications.user_id
notifications.is_read
grievance_history.grievance_id
```

Indexes improve query performance.

However, unnecessary indexes should be avoided because they increase storage requirements and can slow down INSERT/UPDATE operations.

---

# 27. Authentication Data

Passwords must never be stored as plain text.

Incorrect:

```text
password = "mypassword123"
```

Correct:

```text
password_hash = "<secure password hash>"
```

The backend should hash passwords using a secure password hashing algorithm such as:

```text
Argon2
bcrypt
```

Authentication should be handled by the backend.

---

# 28. Authorization

NIVARAN should implement role-based access control.

Example:

### User

Can:

* Submit grievance
* View own grievances
* View grievance status
* Add information
* View notifications
* Reopen grievance where permitted

### Manager

Can:

* View assigned grievances
* Review grievances
* Assign/forward grievances
* Add remarks
* Update status

### Associate Dean

Can:

* Review escalated grievances
* Add remarks
* Forward grievances
* Update status
* Resolve grievances where authorized

### Assistant Dean

Can:

* Review assigned grievances
* Add remarks
* Process grievances
* Forward grievances

### Dean R&D

Can:

* Monitor high-level grievance activity
* Review escalated grievances
* Monitor authorities
* Handle higher-level decisions

Actual permissions should be enforced by the backend rather than relying on frontend page visibility.

---

# 29. Grievance Submission Workflow

When a user submits a grievance:

```text
1. User fills grievance form
        ↓
2. Frontend validates input
        ↓
3. Frontend sends request to backend
        ↓
4. Backend validates request
        ↓
5. Backend creates grievance record
        ↓
6. Unique grievance number is generated
        ↓
7. Initial status = Submitted
        ↓
8. Grievance history record created
        ↓
9. Notification generated
        ↓
10. Response sent to frontend
```

---

# 30. Grievance Assignment Workflow

```text
Grievance
    ↓
Manager reviews grievance
    ↓
Selects authority
    ↓
Assignment record created
    ↓
Current assignee updated
    ↓
History record created
    ↓
Notification generated
```

---

# 31. Grievance Resolution Workflow

```text
Submitted
    ↓
Under Review
    ↓
Assigned
    ↓
In Progress
    ↓
Resolved
    ↓
Closed
```

At every major transition:

1. Grievance status is updated.
2. History record is inserted.
3. Relevant authority/user is notified.

---

# 32. Database Transaction Management

Multiple related operations should be performed inside a transaction.

For example, when assigning a grievance:

```text
BEGIN TRANSACTION

1. Create assignment
2. Update grievance assignee
3. Update grievance status
4. Insert history record
5. Create notification

COMMIT
```

If any operation fails:

```text
ROLLBACK
```

This prevents inconsistent database states.

---

# 33. Example SQL – Create Database

```sql
CREATE DATABASE nivaran_db;
```

---

# 34. Example SQL – Create Roles Table

```sql
CREATE TABLE roles (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    role_name VARCHAR(50) NOT NULL UNIQUE,
    description TEXT
);
```

For PostgreSQL, `BIGSERIAL` or an identity column can be used instead of `AUTO_INCREMENT`.

---

# 35. Example SQL – Create Users Table

```sql
CREATE TABLE users (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(150) NOT NULL UNIQUE,
    password_hash VARCHAR(255) NOT NULL,
    role_id BIGINT NOT NULL,
    phone VARCHAR(20),
    department VARCHAR(150),
    designation VARCHAR(150),
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,

    FOREIGN KEY (role_id)
        REFERENCES roles(id)
);
```

---

# 36. Example SQL – Create Categories Table

```sql
CREATE TABLE grievance_categories (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    category_name VARCHAR(100) NOT NULL UNIQUE,
    description TEXT,
    is_active BOOLEAN DEFAULT TRUE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

# 37. Example SQL – Create Grievances Table

```sql
CREATE TABLE grievances (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    grievance_number VARCHAR(30) NOT NULL UNIQUE,
    user_id BIGINT NOT NULL,
    category_id BIGINT,
    subject VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    priority VARCHAR(20) DEFAULT 'Normal',
    status VARCHAR(30) DEFAULT 'Submitted',
    current_assignee_id BIGINT,
    submitted_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    resolved_at TIMESTAMP NULL,
    closed_at TIMESTAMP NULL,

    FOREIGN KEY (user_id)
        REFERENCES users(id),

    FOREIGN KEY (category_id)
        REFERENCES grievance_categories(id),

    FOREIGN KEY (current_assignee_id)
        REFERENCES users(id)
);
```

---

# 38. Example SQL – Insert Roles

```sql
INSERT INTO roles (role_name, description)
VALUES
('User', 'Can submit and track grievances'),
('Manager', 'Manages and processes grievances'),
('Associate Dean', 'Handles assigned and escalated grievances'),
('Assistant Dean', 'Processes assigned grievances'),
('Dean R&D', 'Higher-level grievance authority');
```

---

# 39. Example SQL – Insert Categories

```sql
INSERT INTO grievance_categories
(category_name, description)
VALUES
('Academic', 'Academic related grievances'),
('Examination', 'Examination related grievances'),
('Hostel', 'Hostel related grievances'),
('Fee', 'Fee related grievances'),
('Scholarship', 'Scholarship related grievances'),
('Infrastructure', 'Infrastructure related grievances'),
('Administrative', 'Administrative grievances'),
('Technical', 'Technical related grievances'),
('Library', 'Library related grievances'),
('Other', 'Other grievances');
```

---

# 40. Basic Database Queries

## Get all grievances

```sql
SELECT *
FROM grievances;
```

## Get grievances submitted by a user

```sql
SELECT *
FROM grievances
WHERE user_id = ?;
```

## Get pending grievances

```sql
SELECT *
FROM grievances
WHERE status IN ('Submitted', 'Under Review', 'Assigned', 'In Progress');
```

## Get resolved grievances

```sql
SELECT *
FROM grievances
WHERE status = 'Resolved';
```

## Get grievances by category

```sql
SELECT *
FROM grievances
WHERE category_id = ?;
```

## Get grievances assigned to an authority

```sql
SELECT *
FROM grievances
WHERE current_assignee_id = ?;
```

---

# 41. Dashboard Queries

The database can support dashboard statistics.

### Total grievances

```sql
SELECT COUNT(*) AS total_grievances
FROM grievances;
```

### Pending grievances

```sql
SELECT COUNT(*) AS pending_grievances
FROM grievances
WHERE status NOT IN ('Resolved', 'Closed');
```

### Resolved grievances

```sql
SELECT COUNT(*) AS resolved_grievances
FROM grievances
WHERE status = 'Resolved';
```

### Category-wise grievances

```sql
SELECT
    category_id,
    COUNT(*) AS total
FROM grievances
GROUP BY category_id;
```

### Status-wise grievances

```sql
SELECT
    status,
    COUNT(*) AS total
FROM grievances
GROUP BY status;
```

---

# 42. Data Integrity

Data integrity ensures that information stored in the database remains:

* Accurate
* Consistent
* Valid
* Complete
* Reliable

NIVARAN maintains data integrity through:

* Primary keys
* Foreign keys
* Unique constraints
* NOT NULL constraints
* CHECK constraints
* Transactions
* Backend validation
* Referential integrity

---

# 43. Data Security

The database should follow secure data management practices.

Important measures include:

1. Never store plain-text passwords.
2. Use secure password hashing.
3. Use parameterized SQL queries.
4. Prevent SQL injection.
5. Use HTTPS for API communication.
6. Restrict direct database access.
7. Use least-privilege database accounts.
8. Keep database credentials in environment variables.
9. Maintain regular backups.
10. Log important administrative actions.
11. Protect uploaded documents.
12. Do not expose sensitive database information through APIs.

Example environment variables:

```text
DB_HOST
DB_PORT
DB_NAME
DB_USER
DB_PASSWORD
```

These values should not be hard-coded into frontend files.

---

# 44. SQL Injection Prevention

The backend must use parameterized queries or a secure ORM.

Unsafe:

```sql
SELECT *
FROM users
WHERE email = 'user_input';
```

The actual implementation should use parameter binding.

Example concept:

```text
SELECT * FROM users WHERE email = ?
```

The user-provided value is supplied separately as a parameter.

---

# 45. Backup and Recovery

The NIVARAN database should have a backup strategy.

Recommended:

```text
Daily Backup
      ↓
Weekly Full Backup
      ↓
Secure Backup Storage
```

Backup copies should be stored separately from the main database server.

A recovery plan should also be maintained to restore the database after:

* Hardware failure
* Software failure
* Accidental deletion
* Data corruption
* Security incidents

---

# 46. Audit Trail

The grievance history table acts as an audit trail.

The system should record:

* Who performed the action
* What action was performed
* Previous status
* New status
* Remarks
* Date and time

Example:

```text
Action By: Manager
Action: Forwarded Grievance
Old Status: Under Review
New Status: Forwarded
Time: 2026-08-19 12:30:00
```

This improves transparency and accountability.

---

# 47. AI Integration with Database

The database is designed to support future AI features.

Possible AI features include:

### Automatic Category Classification

Example:

```text
Input:
"My examination result is incorrect."

AI Prediction:
Category = Examination
```

### Priority Prediction

Example:

```text
Input:
"Urgent issue affecting final semester students."

AI Prediction:
Priority = High
```

### Sentiment Analysis

The AI model can identify general sentiment such as:

```text
Positive
Neutral
Negative
```

### Duplicate Grievance Detection

AI can compare new grievances with existing grievances and identify potentially duplicate complaints.

The AI results should be stored separately in the `ai_analysis` table.

---

# 48. Database and AI Workflow

```text
User submits grievance
        ↓
Database stores grievance
        ↓
Backend sends grievance text to AI service
        ↓
AI analyzes grievance
        ↓
Category / Priority / Sentiment predicted
        ↓
AI result stored in database
        ↓
Authority reviews AI suggestion
        ↓
Authority takes final decision
```

The AI output should be treated as an assistance mechanism and not as an unquestionable final decision.

---

# 49. Scalability

The database should be designed so that it can support increasing numbers of:

* Users
* Grievances
* Departments
* Authorities
* Notifications
* Attachments
* AI analysis records

Scalability can be improved through:

* Proper indexing
* Database normalization
* Query optimization
* Pagination
* Connection pooling
* Caching
* Archiving old records
* Database monitoring

---

# 50. Pagination

Large grievance lists should not be fetched completely in one request.

Instead:

```text
Page 1 → 20 grievances
Page 2 → next 20 grievances
Page 3 → next 20 grievances
```

This improves performance.

Example:

```sql
SELECT *
FROM grievances
ORDER BY submitted_at DESC
LIMIT 20 OFFSET 0;
```

---

# 51. Database–Backend Connection

The backend is responsible for connecting the application to the database.

The flow is:

```text
Frontend
   ↓
API Request
   ↓
Backend Controller
   ↓
Service Layer
   ↓
Database Query
   ↓
Database
   ↓
Query Result
   ↓
Backend
   ↓
API Response
   ↓
Frontend
```

The frontend should never contain database credentials.

---

# 52. Database–Frontend Connection

The frontend communicates with the backend through APIs.

Example:

```text
Frontend:
POST /api/grievances
        ↓
Backend:
Validate request
        ↓
Database:
INSERT grievance
        ↓
Backend:
Return response
        ↓
Frontend:
Display success message
```

For viewing grievances:

```text
Frontend
   ↓
GET /api/grievances
   ↓
Backend
   ↓
Database SELECT
   ↓
Backend
   ↓
JSON Response
   ↓
Frontend
```

---

# 53. Suggested Database Folder Structure

The backend/database project can be organized as:

```text
database/
│
├── schema/
│   ├── users.sql
│   ├── roles.sql
│   ├── grievances.sql
│   ├── categories.sql
│   ├── assignments.sql
│   ├── comments.sql
│   ├── history.sql
│   ├── notifications.sql
│   ├── attachments.sql
│   └── ai_analysis.sql
│
├── migrations/
│   ├── 001_initial_schema.sql
│   ├── 002_add_notifications.sql
│   └── 003_add_ai_analysis.sql
│
├── seeds/
│   ├── roles.sql
│   └── categories.sql
│
└── documentation.md
```

---

# 54. Database Migration

Database migrations should be used when the schema changes.

Example:

```text
Migration 001
Initial database structure

Migration 002
Add notification system

Migration 003
Add attachment support

Migration 004
Add AI analysis

Migration 005
Add grievance escalation
```

Migrations allow the database structure to be version controlled.

---

# 55. Important Database Rules

The following rules should always be followed:

1. Every grievance must belong to a valid user.
2. Every grievance should have a unique grievance number.
3. Every grievance should have a valid status.
4. Every assignment must reference a valid authority.
5. Every comment must belong to an existing grievance.
6. Every notification must belong to a valid user.
7. Passwords must never be stored as plain text.
8. Database credentials must never be stored in frontend code.
9. Important grievance actions must be recorded in history.
10. Deleted or modified records should be handled carefully to preserve auditability.

---

# 56. Soft Delete

For important records, permanent deletion should generally be avoided.

Instead of:

```sql
DELETE FROM users;
```

a soft-delete approach can be used:

```text
is_active = FALSE
```

This preserves historical information while preventing the account from being actively used.

---

# 57. Timestamps

Important tables should maintain timestamps such as:

```text
created_at
updated_at
submitted_at
resolved_at
closed_at
```

Timestamps help with:

* Tracking grievance duration
* SLA monitoring
* Audit trail
* Reporting
* Performance analysis

---

# 58. SLA and Response-Time Tracking

The database can later support Service Level Agreement (SLA) monitoring.

Example:

```text
Grievance Submitted
        ↓
Response Deadline
        ↓
Authority Action
        ↓
Resolution
```

The system can calculate:

```text
Resolution Time
= Resolved Time - Submission Time
```

This can be used to identify delayed grievances.

---

# 59. Reporting and Analytics

The database can support reports such as:

* Total grievances
* Pending grievances
* Resolved grievances
* Rejected grievances
* Category-wise grievances
* Department-wise grievances
* Authority-wise workload
* Monthly grievance count
* Average resolution time
* High-priority grievances
* Reopened grievances
* Escalated grievances

These reports can be displayed on administrator dashboards.

---

# 60. Future Database Enhancements

Future versions of NIVARAN may include:

1. Department table
2. University/student details table
3. Escalation table
4. SLA table
5. Feedback/rating table
6. AI model tracking
7. Duplicate grievance detection
8. Advanced analytics
9. Email notification records
10. SMS notification records
11. Document verification
12. Multi-level escalation
13. Complaint satisfaction tracking

---

# 61. Final Database Architecture

The complete conceptual database structure is:

```text
                         ┌──────────────┐
                         │    ROLES     │
                         └──────┬───────┘
                                │
                                │
                         ┌──────▼───────┐
                         │    USERS     │
                         └──────┬───────┘
                                │
                                │ submits
                                ▼
                      ┌───────────────────┐
                      │    GRIEVANCES     │
                      └─────────┬─────────┘
                                │
              ┌─────────────────┼─────────────────┐
              │                 │                 │
              ▼                 ▼                 ▼
      ┌──────────────┐  ┌──────────────┐  ┌──────────────┐
      │  CATEGORIES  │  │  ASSIGNMENTS │  │   COMMENTS   │
      └──────────────┘  └──────────────┘  └──────────────┘
                                │
                                ▼
                       ┌─────────────────┐
                       │ GRIEVANCE       │
                       │ HISTORY         │
                       └─────────────────┘
                                │
                 ┌──────────────┼──────────────┐
                 │              │              │
                 ▼              ▼              ▼
        ┌──────────────┐ ┌──────────────┐ ┌──────────────┐
        │NOTIFICATIONS │ │ ATTACHMENTS  │ │ AI_ANALYSIS  │
        └──────────────┘ └──────────────┘ └──────────────┘
```

---

# 62. Conclusion

The NIVARAN database provides the core data management layer for the complete grievance redressal system.

It is responsible for:

* User management
* Role management
* Authentication support
* Grievance storage
* Category management
* Assignment management
* Status tracking
* Comments and remarks
* Notifications
* Attachments
* Audit history
* AI-assisted analysis
* Reporting and analytics

The database is designed using relational database principles, normalization, primary keys, foreign keys, constraints, indexing, transactions, and security practices.

The overall objective is to provide a **secure, scalable, consistent, and maintainable database system** that can support the current NIVARAN portal as well as future AI-based grievance management features.

---

# 63. Database Module Summary

| Module        | Main Responsibility                    |
| ------------- | -------------------------------------- |
| Users         | Stores user and authority information  |
| Roles         | Defines system roles                   |
| Categories    | Stores grievance categories            |
| Grievances    | Stores submitted grievances            |
| Assignments   | Tracks grievance assignment            |
| Comments      | Stores remarks/comments                |
| History       | Maintains complete grievance lifecycle |
| Notifications | Manages system notifications           |
| Attachments   | Stores uploaded document metadata      |
| AI Analysis   | Stores AI-generated analysis           |
| Reports       | Supports analytics and dashboards      |

---

# 64. Final Data Flow

```text
USER
  ↓
FRONTEND
  ↓
BACKEND API
  ↓
AUTHENTICATION & AUTHORIZATION
  ↓
VALIDATION
  ↓
DATABASE
  ↓
GRIEVANCE RECORD
  ↓
ASSIGNMENT
  ↓
STATUS UPDATE
  ↓
HISTORY
  ↓
NOTIFICATION
  ↓
AI ANALYSIS (Optional)
  ↓
RESOLUTION
  ↓
CLOSURE
```

**Database Responsibility:**
The database serves as the reliable source of truth for all persistent NIVARAN application data and maintains the complete lifecycle and history of grievances from submission to final resolution.
