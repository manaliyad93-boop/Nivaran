# NIVARAN-AI --- Frontend Documentation

**Project:** NIVARAN-AI --- AI-Assisted Grievance Redressal System\
**Document:** Frontend Technical Documentation\
**Version:** 1.0\
**Frontend Type:** Web-based responsive interface\
**Primary Technologies:** HTML5, CSS3, JavaScript-ready integration\
**Primary Frontend Owner:** Manali --- AI, UX/UI, Testing &
Documentation Lead

------------------------------------------------------------------------

## 1. Introduction

NIVARAN-AI is a university grievance redressal platform designed to
digitize grievance submission, tracking, processing, escalation and
resolution.

The frontend provides role-specific interfaces for applicants and
authorized university officials. Its main purpose is to make the
grievance lifecycle easy to understand and operate while keeping the
interface consistent across different roles.

The frontend is designed around the principle:

> **AI-assisted, human-controlled grievance management.**

AI recommendations may assist officials with categorization, subject,
priority, summary and clustering, but official workflow decisions remain
under authorized human control.

------------------------------------------------------------------------

## 2. Frontend Objectives

The frontend is responsible for:

1.  Providing a simple public landing page.
2.  Providing login and sign-up entry points.
3.  Allowing applicants to submit grievances.
4.  Allowing applicants to view their grievance information.
5.  Providing grievance details and status/timeline views.
6.  Providing role-specific dashboards.
7.  Showing incoming and assigned grievances to officials.
8.  Supporting assignment, review, action and escalation interfaces.
9.  Displaying AI-assisted information clearly.
10. Providing responsive layouts for desktop, tablet and mobile devices.
11. Showing validation and user-friendly error messages.
12. Preparing the UI for integration with secure backend REST APIs.

------------------------------------------------------------------------

## 3. Frontend Architecture

The current frontend is organized as a lightweight multi-page web
application.

``` text
NIVARAN-AI
│
├── index.html
├── land.html
├── form.html
├── detail.html
├── load.html
│
├── CSS
│   ├── style.css
│   ├── land.css
│   ├── form.css
│   ├── detail.css
│   ├── load.css
│   └── signup.css
│
└── dashboard
    ├── user.html
    ├── user.css
    ├── manager.html
    ├── manager.css
    ├── assistant.html
    ├── assistant.css
    ├── associate.html
    ├── associate.css
    ├── dean.html
    ├── dean.css
    ├── track.html
    └── track.css
```

> If the project later moves to React, Vue or another SPA framework,
> these screens can be converted into reusable components without
> changing the underlying workflow.

------------------------------------------------------------------------

## 4. Frontend Page Documentation

### 4.1 `index.html` --- Entry / Authentication Page

**Purpose:**\
Acts as the initial entry point of the application and provides access
to the authentication flow.

**Responsibilities:** - Application entry. - Login/access interface. -
Navigation toward relevant user entry points. - Link to registration
where required. - Basic branding of NIVARAN-AI.

**Expected backend integration:** - Login API. - Authentication/session
creation. - Role identification. - Redirect to role-specific dashboard.

------------------------------------------------------------------------

### 4.2 `land.html` --- Public Landing Page

**Purpose:**\
Introduces NIVARAN-AI to visitors before authentication.

**Main sections:** - NIVARAN-AI branding. - Grievance redressal system
introduction. - System benefits. - AI-assisted grievance management
explanation. - Access/login/register actions.

**CSS:** `land.css`

------------------------------------------------------------------------

### 4.3 `form.html` --- Grievance Submission Form

**Purpose:**\
Allows an applicant to submit a new grievance.

**Typical form information:** - Applicant information. - Grievance
subject/title. - Grievance description. - Category or related
information. - Supporting document/file. - Other required grievance
metadata.

**Frontend responsibilities:** - Required-field validation. - Input
format validation. - File validation. - Clear error messages. -
Submission feedback. - Prevention of accidental empty submissions.

**Backend integration:**

``` text
POST /api/grievances
```

After successful submission, the backend should return a unique
grievance ID and submission timestamp.

------------------------------------------------------------------------

### 4.4 `detail.html` --- Grievance Details

**Purpose:**\
Displays detailed information about a particular grievance.

**Expected information:** - Grievance ID. - Subject. - Description. -
Category. - Priority. - Current status. - Submission date/time. -
Assigned authority. - Remarks/actions. - Documents. - Resolution
information. - Timeline/history.

The page should be role-aware so that users see only information
permitted for their role.

**CSS:** `detail.css`

------------------------------------------------------------------------

### 4.5 `track.html` --- Grievance Tracking

**Purpose:**\
Allows an applicant to monitor the progress of a grievance.

**Main features:** - Grievance ID. - Current status. - Current stage. -
Last update. - Timeline. - Escalation information where appropriate. -
Resolution/closure information.

**Status sequence:**

``` text
Submitted
    ↓
Under Review
    ↓
Assigned
    ↓
In Progress
    ↓
Pending / Escalated
    ↓
Resolved
    ↓
Closed
```

The UI should visually distinguish the current stage from completed and
future stages.

**CSS:** `track.css`

------------------------------------------------------------------------

## 5. Role-Based Dashboard Documentation

NIVARAN-AI uses role-based interfaces. The major roles are:

-   Applicant / User
-   Manager
-   Assistant Dean
-   Associate Dean
-   Dean R&D

------------------------------------------------------------------------

### 5.1 Applicant Dashboard --- `user.html`

**CSS:** `user.css`

**Primary goal:**\
Allow applicants to submit, monitor and understand their grievances.

**Main functions:** - Dashboard overview. - Total grievances. -
Submitted grievances. - Pending/in-progress grievances. - Resolved
grievances. - Submit new grievance. - View grievance details. - Track
status. - Access notifications where implemented. - Logout.

The current interface uses a clear dashboard header, navigation,
statistics cards, grievance information and helpful information
sections.

------------------------------------------------------------------------

### 5.2 Manager Dashboard --- `manager.html`

**CSS:** `manager.css`

**Primary goal:**\
Monitor incoming grievances and manage case routing.

**Main functions:** - View incoming grievances. - Review grievance
details. - Inspect AI-assisted suggestions. - Assign grievances. -
Reassign grievances. - Search and filter. - Monitor workload. - View
escalation-related information. - View analytics. - Monitor
notifications.

The Manager is the main operational routing point before cases move
through the higher-authority workflow.

------------------------------------------------------------------------

### 5.3 Assistant Dean Dashboard --- `assistant.html`

**CSS:** `assistant.css`

**Primary goal:**\
Process grievances assigned or authorized at Assistant Dean level.

**Main functions:** - View assigned cases. - Open grievance details. -
Review previous actions. - Add remarks. - Request/provide documents. -
Update status. - Take required action. - Escalate cases where required.

The interface should emphasize active case processing rather than
overall institutional analytics.

------------------------------------------------------------------------

### 5.4 Associate Dean Dashboard --- `associate.html`

**CSS:** `associate.css`

**Primary goal:**\
Handle escalated or referred grievances at the Associate Dean level.

**Main functions:** - View escalated/referred cases. - Review complete
case history. - Review previous remarks/actions. - Examine documents. -
Add decisions/remarks. - Update case status. - Escalate further where
authorized. - Record resolution-related information.

------------------------------------------------------------------------

### 5.5 Dean Dashboard --- `dean.html`

**CSS:** `dean.css`

**Primary goal:**\
Provide institution-level grievance oversight and higher-level decision
support.

**Main functions:** - Overall grievance statistics. - Escalated
grievance visibility. - Department overview. - Status distribution. -
Institutional workload monitoring. - Reports/analytics entry point. -
Review higher-level cases. - Resolution/decision actions where
applicable.

The current design includes sections for statistics, grievance
distribution and department-level overview.

------------------------------------------------------------------------

## 6. Common UI Components

The frontend uses reusable visual patterns across role interfaces.

### Header / Navigation

Common elements: - NIVARAN branding. - Page title/system identity. -
Role identity. - Navigation links. - Logout action.

### Statistics Cards

Used for: - Total grievances. - Pending grievances. - In-progress
cases. - Resolved cases. - Escalated cases.

### Grievance Cards / Tables

Used to display: - Grievance ID. - Subject. - Category. - Date. -
Status. - Priority. - Assignee. - Action/view button.

### Status Badges

Recommended statuses:

  Status         Meaning
  -------------- --------------------------------
  Submitted      Grievance received
  Under Review   Being reviewed
  Assigned       Official assigned
  In Progress    Processing is active
  Pending        Waiting for information/action
  Escalated      Moved to higher authority
  Resolved       Resolution recorded
  Closed         Case completed

### Timeline

The timeline communicates the history of a grievance in chronological
order.

Each event should ideally contain:

``` text
Date/Time
Action
Actor/Role
Status
Remark (if available)
```

------------------------------------------------------------------------

## 7. AI-Assisted UI

The frontend is designed to display AI output without presenting it as a
final administrative decision.

### AI-supported information

The system may provide:

-   Suggested category.
-   Suggested subject.
-   Suggested priority.
-   Short grievance summary.
-   Semantic cluster/similarity information.

### Recommended UI pattern

``` text
AI Suggestion
-------------------------
Category: Academic
Subject: Examination issue
Priority: Medium
Summary: ...

[Accept] [Modify / Review]
```

The final action must remain with the authorized official.

------------------------------------------------------------------------

## 8. Frontend Validation

Validation should occur before sending data to the backend.

### Client-side validation examples

-   Required fields must not be empty.
-   Email fields must use valid email format.
-   Text fields should enforce reasonable length limits.
-   File type and size should be checked.
-   Invalid values should be highlighted.
-   Submission buttons should provide feedback while processing.

Client-side validation improves user experience, but **backend
validation remains mandatory**.

------------------------------------------------------------------------

## 9. Responsive Design

The interface should support:

### Desktop

-   Full dashboard layout.
-   Multi-column cards.
-   Tables.
-   Full navigation.

### Tablet

-   Reduced grid columns.
-   Flexible dashboard sections.
-   Scrollable data tables where required.

### Mobile

-   Single-column layout.
-   Stacked cards.
-   Compact navigation.
-   Touch-friendly buttons.
-   Readable grievance details.
-   No horizontal overflow.

Responsive CSS should use flexible widths, CSS Grid/Flexbox and
appropriate media queries.

------------------------------------------------------------------------

## 10. Frontend-to-Backend Integration

The frontend will communicate with the backend through secure APIs.

``` text
HTML/CSS UI
     │
     │ HTTP/HTTPS
     ▼
REST API
     │
     ▼
Backend Workflow Layer
     │
 ┌───┼──────────┐
 ▼   ▼          ▼
DB   AI/ML   Notifications
```

### Example API interactions

``` text
Login
POST /api/auth/login

Submit grievance
POST /api/grievances

Get own grievances
GET /api/grievances/my

Get grievance details
GET /api/grievances/{id}

Track grievance
GET /api/grievances/{id}/timeline

Update status
PATCH /api/grievances/{id}/status

Add remark
POST /api/grievances/{id}/remarks

Assign grievance
POST /api/grievances/{id}/assign

Escalate grievance
POST /api/grievances/{id}/escalate
```

These endpoints are the proposed integration contract and should be
aligned with the final backend implementation.

------------------------------------------------------------------------

## 11. Authentication and Role-Based Navigation

After successful authentication, the backend should provide the
authenticated user's role.

Example:

``` json
{
  "user_id": 1024,
  "name": "User Name",
  "role": "assistant_dean"
}
```

The frontend then routes the user to the appropriate interface.

``` text
Applicant       → user.html
Manager         → manager.html
Assistant Dean  → assistant.html
Associate Dean  → associate.html
Dean R&D        → dean.html
```

Frontend route protection is useful for user experience, but
authorization must always be enforced by the backend.

------------------------------------------------------------------------

## 12. Accessibility and Usability

The frontend should follow these principles:

-   Use readable font sizes.
-   Maintain sufficient contrast.
-   Provide visible focus states.
-   Use labels for form controls.
-   Do not rely only on color to communicate status.
-   Use meaningful button text.
-   Keep important actions easy to find.
-   Provide clear validation/error messages.
-   Maintain consistent navigation.

------------------------------------------------------------------------

## 13. Frontend Security Considerations

The frontend must not be treated as the security boundary.

Important rules:

1.  Never store passwords in frontend code.
2.  Never place database credentials in HTML/CSS/JavaScript.
3.  Do not trust client-side role information.
4.  Avoid exposing sensitive grievance data unnecessarily.
5.  Use HTTPS in deployment.
6.  Sanitize/display server data safely.
7.  Do not expose private document URLs without backend authorization.
8.  Handle authentication tokens securely according to the final backend
    architecture.

------------------------------------------------------------------------

## 14. Testing Strategy

### Functional testing

Test:

-   Login navigation.
-   Grievance submission.
-   Form validation.
-   File selection.
-   Dashboard loading.
-   Grievance details.
-   Status tracking.
-   Role-specific actions.
-   Escalation screens.
-   Logout.

### UI testing

Test:

-   Desktop.
-   Tablet.
-   Mobile.
-   Different screen widths.
-   Long grievance text.
-   Empty states.
-   Error states.
-   Loading states.

### Role testing

Each role must be tested independently:

  Role             Main Test
  ---------------- -----------------------------------------
  Applicant        Submit and track grievance
  Manager          Review, assign/reassign
  Assistant Dean   Process assigned case
  Associate Dean   Handle escalated case
  Dean             Review higher-level cases and oversight

------------------------------------------------------------------------

## 15. Current Frontend Limitations

The current interface is primarily a frontend prototype/static
implementation.

The following items require backend integration before production use:

-   Real authentication.
-   Real database records.
-   Dynamic grievance counts.
-   Real file upload.
-   Persistent status changes.
-   Notifications.
-   Real assignment.
-   Real escalation.
-   AI API integration.
-   Audit logging.
-   Secure authorization.
-   Live analytics.

Static/sample values used in UI demonstrations must eventually be
replaced by API-driven data.

------------------------------------------------------------------------

## 16. Future Frontend Enhancements

Possible enhancements include:

1.  Real-time notifications.
2.  Advanced search and filters.
3.  Dynamic charts.
4.  Better document preview.
5.  Role-aware navigation generated from permissions.
6.  Accessibility improvements.
7.  Dark mode if required.
8.  Progressive loading and skeleton states.
9.  Advanced AI suggestion panels.
10. Similar grievance visualization.
11. Mobile-first improvements.
12. Centralized reusable UI components.

------------------------------------------------------------------------

## 17. Frontend Definition of Done

The frontend will be considered complete when:

-   [ ] Landing page is functional.
-   [ ] Authentication screens are connected.
-   [ ] Applicant can submit a grievance.
-   [ ] Applicant can view grievance details.
-   [ ] Applicant can track status/timeline.
-   [ ] Manager dashboard is integrated.
-   [ ] Assistant Dean dashboard is integrated.
-   [ ] Associate Dean dashboard is integrated.
-   [ ] Dean dashboard is integrated.
-   [ ] Role-based navigation works.
-   [ ] API data replaces static dashboard values.
-   [ ] Form validation is implemented.
-   [ ] Error/loading/empty states are implemented.
-   [ ] Responsive layouts are verified.
-   [ ] AI suggestions are displayed clearly.
-   [ ] Unauthorized UI actions are hidden where appropriate.
-   [ ] End-to-end workflow is tested with the backend.
-   [ ] Screenshots and documentation are updated.

------------------------------------------------------------------------

## 18. Conclusion

The NIVARAN-AI frontend provides the user-facing layer of the grievance
redressal system. It separates applicant interaction from official
workflow interfaces and provides dedicated dashboards for each
authorized role.

The frontend is intentionally designed so that the same grievance
lifecycle can be represented consistently across submission, review,
assignment, processing, escalation, resolution and closure.

The final production frontend should remain modular, responsive,
accessible and API-driven, with all security-sensitive decisions
enforced by the backend.
