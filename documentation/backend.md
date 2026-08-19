# NIVARAN-AI — BACKEND DOCUMENTATION

## AI-Assisted Grievance Redressal System

**Module:** Backend, API, Authentication, Authorization, Workflow & Integration
**Primary Owner:** Harshit Gupta — Project Lead + Integration Lead + Backend & Database Lead
**Frontend / AI / Testing / Documentation:** Manali Yadav
**Project:** NIVARAN-AI — AI-Assisted Grievance Redressal System

---

# 1. Backend Overview

NIVARAN-AI is an AI-assisted digital grievance redressal system designed to manage the complete lifecycle of grievances submitted by applicants.

The backend acts as the **central processing and control layer** between the frontend, database, AI module and file-storage system.

The backend is responsible for receiving requests from the frontend, validating incoming data, authenticating users, enforcing role-based authorization, applying grievance workflow rules, communicating with the database, integrating AI-assisted functionality, handling assignments and escalations, generating notifications, maintaining audit history and providing dashboard data.

The approved project plan assigns complete responsibility for backend, database, APIs, authentication, authorization, security, integration and deployment to the Backend & Database Lead.

---

# 2. Backend Objectives

The major objectives of the NIVARAN-AI backend are:

1. Provide a secure communication layer between frontend and database.
2. Authenticate registered users.
3. Enforce role-based access control.
4. Manage user accounts and roles.
5. Accept and validate grievance submissions.
6. Generate a unique grievance ID automatically.
7. Store grievance information in the database.
8. Control grievance status transitions.
9. Support grievance categorization.
10. Support assignment of grievances to responsible authorities.
11. Store official remarks and action notes.
12. Manage escalation between authorities.
13. Support grievance-related document uploads.
14. Generate notifications and reminders.
15. Maintain timestamped audit history.
16. Integrate AI-assisted grievance features.
17. Provide dashboard data through APIs.
18. Validate inputs and return useful error messages.
19. Protect sensitive information and restricted APIs.
20. Support database backup and recovery planning.
21. Connect all frontend interfaces with real backend services.
22. Support final deployment and end-to-end system integration.

These responsibilities are directly aligned with the approved team work plan.

---

# 3. Backend Position in System Architecture

NIVARAN-AI follows a layered application architecture.

```text
                         USER
                           |
                           v
                +----------------------+
                |      FRONTEND        |
                |     HTML/CSS/JS      |
                | Forms / Dashboards   |
                | Status / Notifications|
                +----------+-----------+
                           |
                           | HTTP / REST API
                           v
                +----------------------+
                |       BACKEND        |
                |----------------------|
                | Authentication       |
                | Authorization        |
                | Validation           |
                | Business Logic       |
                | Workflow Management  |
                | Assignment           |
                | Escalation           |
                | Notifications        |
                | File Handling        |
                | AI Integration       |
                | Dashboard APIs       |
                +----+------------+----+
                     |            |
                     v            v
              +-----------+  +-----------+
              | DATABASE  |  | AI MODULE |
              +-----------+  +-----------+
                     |
                     v
              Persistent Data
```

The backend therefore acts as the **bridge and control layer** connecting the frontend with persistent data, AI functionality and other supporting services. The project plan specifically requires frontend/backend integration, authentication, APIs and real database-driven dashboards.

---

# 4. Backend Functional Hierarchy

The backend can be logically divided into the following hierarchy:

```text
BACKEND
│
├── 1. Authentication
│   ├── Registration
│   ├── Login
│   ├── Logout
│   └── Current User
│
├── 2. Authorization
│   ├── Applicant
│   ├── Manager
│   ├── Assistant Dean
│   ├── Associate Dean
│   └── Dean R&D
│
├── 3. User Management
│   ├── User information
│   ├── Role management
│   └── Account status
│
├── 4. Grievance Management
│   ├── Create grievance
│   ├── Read grievance
│   ├── Update grievance
│   ├── Generate grievance ID
│   └── Track status
│
├── 5. Workflow Management
│   ├── Categorization
│   ├── Assignment
│   ├── Processing
│   ├── Remarks
│   ├── Escalation
│   ├── Resolution
│   └── Closure
│
├── 6. Document Management
│   ├── Upload
│   ├── Validate
│   ├── Store reference
│   └── Retrieve
│
├── 7. Notification Management
│   ├── Notifications
│   └── Reminders
│
├── 8. AI Integration
│   ├── Category prediction
│   ├── Subject suggestion
│   ├── Priority suggestion
│   └── Summary generation
│
├── 9. Dashboard APIs
│   ├── Manager Dashboard
│   └── Dean Dashboard
│
├── 10. Audit & Logging
│   └── Timestamped activity history
│
├── 11. Security
│   ├── Authentication
│   ├── Authorization
│   ├── Input validation
│   └── Secure file handling
│
└── 12. Deployment & Backup
    ├── Environment configuration
    ├── Deployment
    └── Database backup
```

---

# 5. Backend Request Flow

Every frontend operation should follow a controlled request-response cycle.

```text
User
  |
  v
Frontend Interface
  |
  | HTTP Request
  v
API Endpoint
  |
  v
Authentication Check
  |
  v
Authorization Check
  |
  v
Input Validation
  |
  v
Business Logic
  |
  v
Database / AI / File Service
  |
  v
Process Result
  |
  v
API Response
  |
  v
Frontend
  |
  v
User
```

This architecture prevents the frontend from directly manipulating database records and ensures that business rules are enforced centrally.

---

# 6. Authentication

## 6.1 Purpose

Authentication verifies the identity of the person attempting to access NIVARAN-AI.

A typical authentication process is:

```text
User
 |
 | Email + Password
 v
Login API
 |
 v
Validate Credentials
 |
 +---- Invalid ----> Error Response
 |
 v
Valid Credentials
 |
 v
Authenticated User
 |
 v
Role Identification
 |
 v
Authorized Application Access
```

The backend must ensure that credentials are verified before granting access.

---

# 7. Authorization and Role-Based Access Control

Authentication answers:

**"Who is the user?"**

Authorization answers:

**"What is this user allowed to do?"**

NIVARAN-AI contains the following major roles:

1. Applicant
2. Manager
3. Assistant Dean
4. Associate Dean
5. Dean R&D

The backend must enforce access restrictions for both frontend-protected pages and backend APIs.

### Role hierarchy

```text
                    NIVARAN-AI
                         |
              +----------+----------+
              |                     |
          Applicant             Officials
                                    |
                    +---------------+---------------+
                    |               |               |
                 Manager      Assistant Dean   Associate Dean
                                                    |
                                                    v
                                               Dean R&D
```

### Applicant

Applicant can:

* Register/login.
* Submit a grievance.
* Upload supporting documents.
* View submitted grievances.
* Track grievance status.
* View relevant notifications.
* View grievance updates.

### Manager

Manager can:

* Access assigned grievances.
* Review grievance information.
* Process assigned cases.
* Add official remarks.
* Update permitted workflow information.
* Participate in case management.

### Assistant Dean

Assistant Dean can:

* Review grievances within the assigned workflow.
* Add remarks.
* Take action.
* Escalate grievances where required.

### Associate Dean

Associate Dean can:

* Review escalated grievances.
* Add remarks.
* Take action.
* Continue escalation when required.

### Dean R&D

Dean R&D provides:

* Leadership-level visibility.
* Escalation handling.
* Final-level grievance oversight.
* Dashboard-level information.

---

# 8. Grievance Management

The grievance module is the central business module of NIVARAN-AI.

The backend must support:

1. Grievance submission.
2. Unique grievance ID generation.
3. Applicant association.
4. Subject storage.
5. Description storage.
6. Category storage.
7. Priority storage.
8. Status management.
9. AI-generated suggestions.
10. Assignment.
11. Remarks.
12. Escalation.
13. Document association.
14. Resolution.
15. Closure.
16. Audit history.

## The approved plan explicitly defines the grievance lifecycle and associated data.

# 9. Unique Grievance ID Generation

Every grievance must receive a unique identifier.

```text
Applicant submits grievance
          |
          v
Backend receives request
          |
          v
Validate grievance data
          |
          v
Generate unique grievance ID
          |
          v
Create grievance record
          |
          v
Store in database
          |
          v
Return grievance ID
          |
          v
Show ID to applicant
```

The grievance ID should uniquely identify the grievance throughout its entire lifecycle.

---

# 10. Grievance Lifecycle

The backend must enforce a controlled workflow.

```text
SUBMITTED
    |
    v
CATEGORIZED
    |
    v
ASSIGNED
    |
    v
UNDER REVIEW
    |
    v
ACTION TAKEN
    |
    +-------> ESCALATED
    |             |
    |             v
    |        NEXT AUTHORITY
    |             |
    |             v
    +----------> RESOLVED
                  |
                  v
                CLOSED
```

The approved project workflow explicitly covers submission, categorization, assignment, processing, remarks, escalation, resolution and closure.

---

# 11. Status Transition Control

Users should not be allowed to change a grievance to any arbitrary status.

The backend should verify:

```text
Current Status
      |
      v
Requested New Status
      |
      v
Is Transition Valid?
   /           \
 Yes            No
 |               |
 v               v
Update       Reject Request
Database     + Error Message
```

For example:

```text
SUBMITTED
    ↓
CATEGORIZED
    ↓
ASSIGNED
    ↓
UNDER_REVIEW
    ↓
ACTION_TAKEN
    ↓
RESOLVED
    ↓
CLOSED
```

Escalation should be treated as a controlled workflow action rather than an unrestricted status change.

---

# 12. Assignment Management

Assignment determines which responsible person or authority handles a grievance.

```text
Grievance
    |
    v
Manager / Authorized Authority
    |
    v
Select Responsible User
    |
    v
Create Assignment
    |
    v
Store Assignment
    |
    v
Notify Assigned User
    |
    v
Case Processing
```

The assignment record should maintain:

* Grievance ID
* Assigned user
* Assigning user
* Assignment time
* Optional deadline
* Assignment status

---

# 13. Remarks and Actions

Officials should be able to add official remarks and action notes.

```text
Open Grievance
      |
      v
Review Case
      |
      v
Add Remark / Action
      |
      v
Validate User Permission
      |
      v
Store Remark
      |
      v
Create Audit Entry
      |
      v
Update Notification
```

Remarks provide a traceable record of decisions and actions taken during grievance processing.

---

# 14. Escalation Management

NIVARAN-AI defines the following escalation path:

```text
Assistant Dean
      |
      | Escalation
      v
Associate Dean
      |
      | Escalation
      v
Dean R&D
```

The backend should store every escalation event separately.

Each escalation should contain:

* Grievance ID
* Previous authority
* New authority
* User initiating escalation
* Reason
* Timestamp

This ensures that the complete escalation history remains traceable.

---

# 15. Document Management

The system supports grievance-related document handling.

The backend should provide:

1. File upload.
2. File type validation.
3. File size validation.
4. Secure storage reference.
5. Database metadata storage.
6. Authorized file retrieval.
7. Association with the relevant grievance.
8. Uploader identification.
9. Upload timestamp.

Flow:

```text
User Selects File
       |
       v
Upload Request
       |
       v
Authentication
       |
       v
Authorization
       |
       v
File Validation
       |
       +---- Invalid ----> Reject
       |
       v
Secure Storage
       |
       v
Store Metadata
       |
       v
Link File to Grievance
       |
       v
Success Response
```

The approved plan specifically requires secure file upload, storage/reference and retrieval.

---

# 16. Notification Management

Notifications keep users informed about important grievance events.

Notifications may be generated when:

* A grievance is submitted.
* A grievance is assigned.
* A status changes.
* A remark is added.
* A grievance is escalated.
* A grievance is resolved.
* A reminder is due.
* A new action is required.

Flow:

```text
System Event
     |
     v
Notification Generator
     |
     v
Identify Recipient
     |
     v
Create Notification
     |
     v
Store Notification
     |
     v
Display to User
```

---

# 17. AI Integration

NIVARAN-AI includes AI-assisted grievance functionality.

The planned AI features are:

1. Category prediction.
2. Subject suggestion.
3. Priority suggestion.
4. Short summary generation.
5. Future readiness for similar grievance detection.

These features are explicitly defined in the team plan.

### AI flow

```text
Grievance Description
        |
        v
Frontend
        |
        v
Backend API
        |
        v
AI Module / AI Service
        |
        +------> Category Suggestion
        |
        +------> Subject Suggestion
        |
        +------> Priority Suggestion
        |
        +------> Short Summary
        |
        v
Backend
        |
        v
Frontend
        |
        v
Human Review / Confirmation
```

AI output should be treated as **assistance**, not as an unquestionable final decision. Human review and confirmation remain part of the workflow.

---

# 18. API Layer

The API layer provides communication between frontend and backend.

General flow:

```text
Frontend
   |
   | HTTP Request
   v
API Endpoint
   |
   v
Controller / Route
   |
   v
Validation
   |
   v
Business Logic
   |
   v
Database / AI / Storage
   |
   v
Response
   |
   v
Frontend
```

---

# 19. Recommended API Structure

The following API structure may be used as a documentation-level design.

## Authentication APIs

```text
POST   /api/auth/register
POST   /api/auth/login
POST   /api/auth/logout
GET    /api/auth/me
```

## Grievance APIs

```text
POST   /api/grievances
GET    /api/grievances
GET    /api/grievances/{id}
PUT    /api/grievances/{id}
```

## Status APIs

```text
GET    /api/grievances/{id}/status
PUT    /api/grievances/{id}/status
```

## Assignment APIs

```text
POST   /api/grievances/{id}/assign
GET    /api/grievances/{id}/assignments
```

## Remarks APIs

```text
POST   /api/grievances/{id}/remarks
GET    /api/grievances/{id}/remarks
```

## Escalation APIs

```text
POST   /api/grievances/{id}/escalate
GET    /api/grievances/{id}/escalations
```

## Document APIs

```text
POST   /api/grievances/{id}/documents
GET    /api/grievances/{id}/documents
```

## Notification APIs

```text
GET    /api/notifications
PUT    /api/notifications/{id}/read
```

## Dashboard APIs

```text
GET    /api/dashboard/manager
GET    /api/dashboard/dean
```

**Implementation Note:** These are recommended documentation-level API routes. The final route names should match the actual backend implementation.

---

# 20. Dashboard Backend

Dashboard values must be generated from live database records rather than static values.

The backend may provide:

* Total grievances.
* Pending grievances.
* Resolved grievances.
* Closed grievances.
* Escalated grievances.
* Category-wise counts.
* Status-wise counts.
* Assignment information.
* Recent activity.

Flow:

```text
Dashboard Request
       |
       v
Authentication
       |
       v
Role Authorization
       |
       v
Dashboard API
       |
       v
Database Queries
       |
       v
Aggregate / Process Data
       |
       v
JSON Response
       |
       v
Dashboard UI
```

The project plan explicitly requires Manager and Dean dashboard data to be connected to real database records.

---

# 21. Validation

Backend validation is mandatory before data is stored or processed.

The backend should validate:

* Required fields.
* Email format.
* User role.
* Grievance ID.
* Grievance status.
* Assignment.
* File type.
* File size.
* Authorization.
* Workflow conditions.
* Request structure.

Flow:

```text
Incoming Request
       |
       v
Schema / Input Validation
       |
   +---+---+
   |       |
Valid    Invalid
   |       |
   v       v
Process   Reject
Request   Request
   |       |
   v       v
Database Error Response
```

---

# 22. Error Handling

The backend should return structured and meaningful error responses.

Example:

```json
{
  "success": false,
  "message": "Invalid grievance data",
  "errors": {
    "subject": "Subject is required"
  }
}
```

Possible errors include:

```text
Invalid credentials
Unauthorized access
Invalid role
Missing required field
Invalid grievance ID
Invalid status transition
Invalid assignment
Invalid file type
File size exceeded
Database error
Internal server error
```

The frontend should display useful messages, but the backend must remain responsible for enforcing validation and business rules.

---

# 23. Security Architecture

Security is an essential backend responsibility.

The backend should implement:

1. Secure password hashing.
2. Authentication.
3. Role-based authorization.
4. Protected APIs.
5. Input validation.
6. Secure file handling.
7. Restricted access to sensitive data.
8. Safe handling of environment variables and secrets.
9. Audit logging.
10. Database backup.
11. Protection against unauthorized role access.

Security flow:

```text
Client Request
      |
      v
Authentication
      |
      v
Authorization
      |
      v
Input Validation
      |
      v
Business Rules
      |
      v
Database Operation
      |
      v
Audit Logging
      |
      v
Response
```

---

# 24. Audit Logging

Important backend operations should create audit records.

Examples:

* Login.
* Grievance creation.
* Assignment.
* Status change.
* Remark addition.
* Escalation.
* Document upload.
* Resolution.
* Closure.

Flow:

```text
Important Action
      |
      v
Perform Operation
      |
      v
Create Audit Record
      |
      v
Store Timestamp
      |
      v
Maintain History
```

Audit logs provide accountability and traceability.

---

# 25. Frontend–Backend Integration

The frontend developed by Manali communicates with the backend through APIs.

```text
                FRONTEND
                   |
                   |
             API Request
                   |
                   v
               BACKEND
                   |
        +----------+----------+
        |                     |
        v                     v
    DATABASE              AI MODULE
        |                     |
        +----------+----------+
                   |
              API Response
                   |
                   v
                FRONTEND
```

For every integrated feature, the following should be clearly defined:

1. API endpoint.
2. HTTP method.
3. Request format.
4. Authentication requirement.
5. Required role.
6. Response format.
7. Error response.
8. Loading state.
9. Success state.

This integration responsibility is explicitly defined in the project work plan.

---

# 26. Complete Grievance Processing Flow

```text
                 APPLICANT
                     |
                     v
            Submit Grievance
                     |
                     v
             Backend Validation
                     |
                     v
          Generate Grievance ID
                     |
                     v
             Store in Database
                     |
                     v
           AI-Assisted Analysis
          /       |       |       \
         v        v       v        v
     Category   Subject Priority  Summary
          \       |       |       /
                   v
             Human Confirmation
                   |
                   v
              Assignment
                   |
                   v
             Under Review
                   |
                   v
              Action Taken
                   |
          +--------+--------+
          |                 |
      Resolution        Escalation
          |                 |
          |          +------+------+
          |          |             |
          |    Associate Dean   Dean R&D
          |          |             |
          +----------+-------------+
                     |
                     v
                  Resolved
                     |
                     v
                   Closed
```

---

# 27. Backend Development Sequence

## Phase 1 — Requirements & Architecture

* Freeze requirements.
* Finalize grievance workflow.
* Finalize roles.
* Plan database.
* Define API structure.
* Establish repository.
* Review frontend flow.

## Phase 2 — Backend Foundation

* Implement database connectivity.
* Implement users and roles.
* Implement grievance model.
* Implement unique grievance ID.
* Implement status logic.
* Implement core APIs.

## Phase 3 — Workflow Implementation

* Implement assignment.
* Implement remarks.
* Implement escalation.
* Implement documents.
* Implement audit logs.
* Implement Manager Dashboard APIs.
* Integrate status tracking.
* Support Assistant Dean and Associate Dean workflow.

## Phase 4 — AI Integration

* Category prediction.
* Subject suggestion.
* Priority suggestion.
* Short summary.
* Backend/API integration with AI module.

## Phase 5 — Notifications, Security & Integration

* Notification backend.
* File upload.
* Security controls.
* Database backup.
* End-to-end frontend/backend integration.

## Phase 6 — Testing & Documentation

* Dummy grievances.
* Validation testing.
* Error testing.
* Backend/API defect fixing.
* Integration testing.
* Documentation support.

## Phase 7 — Final Dashboard & Deployment

* Dean Dashboard integration.
* Final API integration.
* Deployment.
* Authentication verification.
* File upload verification.
* End-to-end workflow verification.

This sequence follows the development phases specified in the approved plan.

---

# 28. Backend Testing Strategy

Backend testing should cover:

### Authentication Testing

* Valid login.
* Invalid login.
* Invalid password.
* Unauthorized access.
* Logout.

### Authorization Testing

* Applicant accessing applicant resources.
* Manager accessing assigned cases.
* Assistant Dean accessing permitted workflow.
* Associate Dean accessing escalated cases.
* Dean R&D accessing leadership-level data.
* Unauthorized role access rejection.

### Grievance Testing

* Create grievance.
* Generate unique ID.
* Retrieve grievance.
* Update permitted information.
* Track status.

### Workflow Testing

* Valid status transition.
* Invalid status transition.
* Assignment.
* Remarks.
* Escalation.
* Resolution.
* Closure.

### File Testing

* Valid file.
* Invalid file type.
* Oversized file.
* Unauthorized file access.

### API Testing

* Valid request.
* Invalid request.
* Missing authentication.
* Invalid role.
* Invalid ID.
* Database failure.

---

# 29. Deployment Architecture

The deployment environment should contain:

```text
                    USERS
                      |
                      v
                 FRONTEND
                      |
                      v
                BACKEND/API
                 /       \
                /         \
               v           v
          DATABASE       AI SERVICE
               |
               v
        Persistent Records
```

Before deployment, verify:

1. Backend configuration.
2. Database connection.
3. Environment variables.
4. Authentication.
5. Authorization.
6. API endpoints.
7. File upload.
8. Notifications.
9. Dashboard APIs.
10. AI integration.
11. Database backup.
12. Complete grievance workflow.

---

# 30. Backend Definition of Done

The backend module is considered complete when:

* Users can authenticate securely.
* Role-based authorization is enforced.
* Grievances can be submitted through APIs.
* Unique grievance IDs are generated.
* Grievances are stored in the database.
* Status transitions follow defined workflow rules.
* Assignments are stored.
* Remarks are stored.
* Escalation history is stored.
* Documents are securely handled.
* Notifications/reminders are supported.
* Audit logs record important actions.
* Dashboard APIs use live database data.
* AI integration works where required.
* Input validation is implemented.
* Unauthorized access is blocked.
* Database backup is configured.
* Frontend and backend are connected.
* Deployed system works end-to-end.

These completion requirements correspond to the final definition of done in the project plan.

---

# 31. Backend Responsibility Hierarchy

```text
BACKEND & INTEGRATION LEAD
│
├── Requirements & Workflow
│
├── Backend Architecture
│
├── Authentication
│
├── Authorization
│
├── APIs
│
├── Grievance Management
│
├── Assignment
│
├── Remarks
│
├── Escalation
│
├── Document Handling
│
├── Notifications
│
├── Audit Logging
│
├── Dashboard Integration
│
├── AI Backend Integration
│
├── Database Integration
│
├── Security
│
├── Backup
│
└── Deployment
```

---

# 32. Final Backend Architecture

```text
                         USER
                           |
                           v
                 +-------------------+
                 |     FRONTEND      |
                 | Forms / Dashboards|
                 | Status / Alerts   |
                 +---------+---------+
                           |
                       REST APIs
                           |
                           v
        +-----------------------------------------+
        |                 BACKEND                 |
        |-----------------------------------------|
        | Authentication                          |
        | Authorization                           |
        | Validation                              |
        | Business Logic                          |
        | Grievance Workflow                      |
        | Assignment                              |
        | Remarks                                 |
        | Escalation                              |
        | Notifications                           |
        | File Handling                           |
        | AI Integration                          |
        | Dashboard APIs                          |
        | Audit Logging                            |
        | Security                                |
        +-------------+---------------+-----------+
                      |               |
                      v               v
             +---------------+   +-------------+
             |   DATABASE    |   | AI MODULE   |
             |---------------|   |-------------|
             | Users         |   | Category    |
             | Roles         |   | Subject     |
             | Grievances    |   | Priority    |
             | Assignments   |   | Summary     |
             | Remarks       |   +-------------+
             | Escalations   |
             | Documents     |
             | Notifications |
             | Audit Logs    |
             +---------------+
```

---

# 33. Final Summary

The NIVARAN-AI backend functions as the central technical backbone of the complete grievance redressal system.

It connects:

**Frontend → APIs → Business Logic → Database → AI Module → Notifications → Users**

while enforcing:

**Authentication + Authorization + Validation + Workflow Control + Security + Auditability**

The backend therefore ensures that NIVARAN-AI operates as an integrated, secure and traceable grievance management platform rather than a collection of independent frontend pages.
