# HireD Buildathon – Technical Guidelines

**Document Type:** Technical Guidelines  
**Project:** HireD – Phase 1 MVP  
**Applies To:** Admin Portal, B2B Portal, Driver App, Backend & Database  
**Status:** Mandatory for all teams

---

## 1. Purpose

This document defines the technical implementation standards that teams must follow while building the HireD product during the Buildathon.

The PRD defines **what the product must do**. These Technical Guidelines define the minimum standards for **how the product should be built, structured, documented, and submitted**.

Teams are free to choose suitable technologies where this document does not specify a mandatory choice.

The implementation must remain aligned with the PRD and the official Buildathon Guidelines.

---

# 2. Mandatory Technical Requirements

The following requirements are mandatory.

### 2.1 Backend Architecture

The backend **must use a Modular Monolithic Architecture**.

The backend should be developed as one deployable application while maintaining clear internal modules based on business domains.

Do not implement the backend as a collection of independently deployed microservices for this Buildathon unless explicitly approved by the organizers.

### 2.2 Database

The primary application database **must be SQL-based**.

Acceptable examples include:

- PostgreSQL
- MySQL
- Microsoft SQL Server
- SQLite
- Oracle
- Other suitable relational SQL databases

NoSQL databases such as MongoDB, DynamoDB, Cassandra, or CouchDB must not be used as the primary application database.

Supporting technologies such as Redis may be used for appropriate purposes such as caching, queues, or temporary data.

### 2.3 Database Design Submission

The database design must be prepared and submitted **within the first 2 days of the Buildathon**.

The database documentation must include, where applicable:

- Entities
- Tables
- Columns
- Data types
- Primary keys
- Foreign keys
- Relationships
- Constraints
- Cardinality
- Indexes
- Normalization decisions
- ER Diagram
- Important database design decisions

### 2.4 Application Surfaces

The project must contain:

- Admin Portal
- B2B Portal
- Driver App
- Backend / Server-side application

Cross-platform mobile development is preferred for the Driver App.

---

# 3. UI / Brand Guidelines

## 3.1 Design Reference

The following Admin Portal is provided as the **design and colour-theme reference**:

**Admin UI Reference:**  
https://lovable.dev/preview/yxgpq9lNpO4DeQMIkkVWxGJHxU2gRPGb

The exact implementation may differ according to the team's technology and UX decisions, but the overall visual identity must remain consistent with the provided reference.

## 3.2 Theme Consistency

The same visual language must be followed across:

- Admin Portal
- B2B Portal
- Driver App

Teams must maintain consistency in:

- Primary colour
- Secondary colour
- Background colours
- Text colours
- Button styles
- Input styles
- Status indicators
- Cards
- Navigation
- Typography
- Border radius
- Spacing
- Icons
- Error/success states

The B2B Portal and Driver App should feel like parts of the same product rather than three unrelated applications.

## 3.3 Logo

The official HireD logo will be provided by the organizers.

Teams must use the provided logo instead of creating a replacement logo.

Do not modify the logo's proportions, shape, or visual identity without approval.

---

# 4. Repository Structure

The root repository **must follow this structure**:

```text
project-root/
│
├── admin-portal/
│
├── b2b-portal/
│
├── driver-app/
│
├── backend/
│
├── docs/
│
└── README.md
```

> `backend/` is included because the project requires a dedicated backend/server-side application.

Each application must remain independently understandable and maintainable.

## 4.1 Admin Portal

```text
admin-portal/
├── src/
├── public/
├── tests/
├── package.json
└── README.md
```

The exact internal structure may depend on the chosen framework, but business logic, UI components, API communication, and utilities should remain logically separated.

## 4.2 B2B Portal

```text
b2b-portal/
├── src/
├── public/
├── tests/
├── package.json
└── README.md
```

## 4.3 Driver App

```text
driver-app/
├── src/
├── assets/
├── tests/
├── package.json
└── README.md
```

The exact structure may differ for Flutter, React Native, .NET MAUI, or another mobile framework.

## 4.4 Backend

```text
backend/
├── src/
├── tests/
├── migrations/
├── seed/
├── package.json
└── README.md
```

---

# 5. Backend Architecture – Modular Monolith

## 5.1 Mandatory Architecture

The backend must follow a **Modular Monolithic Architecture**.

A recommended structure is:

```text
backend/
└── src/
    ├── modules/
    │   ├── auth/
    │   ├── users/
    │   ├── companies/
    │   ├── orders/
    │   ├── trips/
    │   ├── drivers/
    │   ├── expenses/
    │   ├── payroll/
    │   ├── wallet/
    │   ├── invoices/
    │   ├── reports/
    │   └── notifications/
    │
    ├── shared/
    │   ├── database/
    │   ├── middleware/
    │   ├── errors/
    │   ├── utils/
    │   ├── constants/
    │   └── types/
    │
    ├── config/
    ├── routes/
    └── app.*
```

The exact module names may be adjusted based on the final implementation, but business domains should remain clearly separated.

## 5.2 Module Independence

Each module should ideally contain its own:

```text
module/
├── controller/
├── service/
├── repository/
├── model/
├── routes/
├── validation/
└── types/
```

Not every module must contain every directory. Only introduce layers that are actually required.

## 5.3 Recommended Request Flow

A typical API request should follow a predictable flow:

```text
Client
  ↓
Route
  ↓
Authentication / Authorization
  ↓
Validation
  ↓
Controller
  ↓
Service / Business Logic
  ↓
Repository / Data Access
  ↓
SQL Database
```

Business rules should not be placed directly inside route handlers.

Controllers should remain thin and delegate business logic to services.

Database queries should not be scattered throughout controllers.

---

# 6. Business Module Boundaries

Based on the PRD, the following domains should be considered when designing the backend:

| Module | Responsibility |
|---|---|
| Auth | Login, authentication, token/session handling |
| Users/Admins | Admin accounts and user information |
| Companies | B2B company management |
| Orders | Order creation, acceptance and rejection |
| Trips | Trip lifecycle and driver assignment |
| Drivers | Driver profiles, onboarding and availability |
| Documents | Driver documents and secure file references |
| Expenses | Trip expense records |
| Payroll | Company payout split and trip payroll |
| Wallet | Driver wallet and wallet transactions |
| Invoices | Month-end statements and two-person verification |
| Reports | Admin reporting and analytics |
| Notifications | Email or other supported notifications |

The final module boundaries must be documented in `docs/architecture.md`.

---

# 7. API Guidelines

## 7.1 API Style

The backend should expose a consistent HTTP API.

RESTful API design is recommended unless there is a documented reason to use another approach.

Example:

```text
GET    /api/v1/orders
GET    /api/v1/orders/:id
POST   /api/v1/orders
PATCH  /api/v1/orders/:id
POST   /api/v1/orders/:id/accept
POST   /api/v1/orders/:id/reject
```

## 7.2 API Versioning

Use API versioning where appropriate:

```text
/api/v1/...
```

## 7.3 Consistent Response Format

Successful responses should follow a consistent structure.

Example:

```json
{
  "success": true,
  "data": {},
  "message": "Order created successfully"
}
```

Error responses should also be consistent:

```json
{
  "success": false,
  "error": {
    "code": "ORDER_NOT_FOUND",
    "message": "Order not found"
  }
}
```

The exact response contract may vary, but consistency across all APIs is required.

## 7.4 HTTP Status Codes

Use appropriate HTTP status codes.

Examples:

- `200` – Successful request
- `201` – Resource created
- `204` – Successful request with no response body
- `400` – Invalid request
- `401` – Unauthenticated
- `403` – Unauthorized
- `404` – Resource not found
- `409` – Conflict
- `422` – Validation failure
- `500` – Internal server error

## 7.5 API Documentation

All implemented APIs must be documented.

The documentation should include:

- Endpoint
- HTTP method
- Authentication requirement
- Request parameters
- Request body
- Response body
- Status codes
- Validation rules
- Error cases
- Example request
- Example response

Swagger/OpenAPI is recommended.

---

# 8. Database Guidelines

## 8.1 Relational Design

The database should be designed around the business entities defined by the PRD.

The high-level PRD data model includes:

- Company
- Order
- Trip
- Driver
- Expense
- Wallet Transaction
- Trip Payroll Record
- Statement/Invoice

The final database may contain additional supporting entities required by the implementation.

## 8.2 Primary Keys

Every table should have a clearly defined primary key.

Use one consistent ID strategy across the project.

The selected strategy must be documented.

## 8.3 Foreign Keys

Relationships between entities must use proper foreign-key constraints where appropriate.

Do not rely only on application-level assumptions for critical relationships.

## 8.4 Indexing

Indexes should be created for frequently queried fields.

Examples may include:

- Order status
- Company ID
- Driver ID
- Trip status
- Trip date
- Invoice status
- Wallet driver ID

Do not create indexes blindly. Important indexing decisions should be documented.

## 8.5 Data Integrity

Use database constraints where appropriate:

- `NOT NULL`
- `UNIQUE`
- `CHECK`
- Foreign keys
- Appropriate defaults

Critical financial values must use suitable numeric/decimal database types rather than floating-point types where precision matters.

---

# 9. Transaction and Payout Rules

The PRD requires the driver payout to be credited when a trip closes.

The implementation must protect this operation against duplicate wallet credits.

The following conceptual operation should be treated as an atomic business transaction:

```text
Trip Close
   ↓
Validate OTP
   ↓
Validate required expenses
   ↓
Close Trip
   ↓
Calculate Driver Share
   ↓
Create Payroll Record
   ↓
Create Wallet Transaction
   ↓
Update Wallet Balance
```

If the chosen database/architecture supports transactions for these operations, use them.

The implementation must also prevent duplicate processing if the close request is retried.

The applied company payout rate must be stored with the trip/payroll record so that later rate changes do not modify historical calculations.

---

# 10. Naming Conventions

Every team must select and document naming conventions before or during implementation.

The conventions must be followed consistently throughout the project.

Create:

```text
docs/naming-conventions.md
```

## 10.1 Recommended Naming Standards

### Folders

Use:

```text
kebab-case
```

Example:

```text
driver-app/
user-management/
```

### JavaScript / TypeScript Variables

Use:

```text
camelCase
```

Example:

```ts
const driverId = "DRV001";
const tripAmount = 10000;
```

### Functions

Use descriptive `camelCase` names:

```ts
createTrip()
assignDriver()
calculateDriverPayout()
closeTrip()
```

### Classes

Use:

```text
PascalCase
```

Example:

```ts
TripService
WalletService
InvoiceController
```

### Database Tables

Choose one convention and use it consistently.

Recommended:

```text
snake_case
```

Example:

```text
companies
driver_documents
trip_payroll_records
wallet_transactions
```

### Database Columns

Recommended:

```text
snake_case
```

Example:

```text
company_id
driver_id
created_at
updated_at
```

### API Routes

Use:

```text
kebab-case
```

Example:

```text
/api/v1/driver-documents
/api/v1/trip-reports
```

Do not mix:

```text
/driverDocuments
/driver_documents
/driver-documents
```

without a documented reason.

---

# 11. Function and Code Quality Guidelines

## 11.1 Single Responsibility

Functions should perform one clear responsibility.

Avoid:

```text
One function → validate input → query database → calculate payroll → send email → update wallet
```

Prefer separating responsibilities into appropriate services/functions.

## 11.2 Function Naming

Function names should clearly communicate the action.

Good:

```text
calculateDriverShare()
generateMonthlyStatement()
assignDriverToTrip()
verifyInvoice()
```

Avoid vague names:

```text
processData()
handleThing()
doStuff()
```

## 11.3 Reusability

Common functionality should be reused rather than duplicated.

Examples:

- Validation
- Authentication
- Error handling
- API response formatting
- Date utilities
- Permission checks
- Database utilities

## 11.4 Avoid Overengineering

The project should be maintainable without unnecessary abstractions.

Do not introduce:

- Unnecessary microservices
- Excessive design patterns
- Unused libraries
- Complex abstractions without a business requirement

Technical decisions should solve a real project problem.

---

# 12. Authentication and Authorization

Authentication must be implemented for all protected application areas.

The implementation should clearly distinguish:

- Admin
- B2B Partner
- Driver

Access must be restricted according to the PRD.

### B2B Partner

A partner must only be able to access data belonging to their own company.

### Driver

A driver must only be able to access their own:

- Profile
- Documents
- Availability
- Assigned trips
- Expenses
- Trip history
- Wallet

### Admin

Phase 1 Admin accounts have the same access level according to the PRD.

The system must support at least two Admin accounts for two-person invoice verification.

---

# 13. Security Guidelines

Security must be considered throughout development.

At minimum, teams must address:

- Secure password hashing
- Authentication
- Authorization
- Input validation
- Request validation
- SQL injection prevention
- API security
- Access control
- Token/session security
- Secure file upload
- Environment variable protection
- Secret management
- Error handling
- Rate limiting where appropriate
- CORS configuration
- HTTPS in deployed environments
- Protection against common web vulnerabilities

## 13.1 Secrets

Never commit:

```text
.env
.env.local
API keys
database passwords
JWT secrets
cloud credentials
private keys
```

Use environment variables.

Provide:

```text
.env.example
```

with placeholder values.

Example:

```env
DATABASE_URL=
JWT_SECRET=
STORAGE_BUCKET=
STORAGE_ACCESS_KEY=
STORAGE_SECRET_KEY=
```

Do not put real credentials in the repository.

---

# 14. File Upload Security

The Driver App requires document uploads and selfie uploads.

Uploaded files should be handled securely.

Teams should consider:

- File type validation
- File size limits
- Secure storage
- Access control
- Safe file naming
- Avoiding executable uploads
- Authentication before accessing private documents
- Preventing unauthorized document access

Driver documents should not be publicly accessible unless explicitly required.

---

# 15. Environment Configuration

Each application must document its environment configuration.

At minimum, provide:

```text
.env.example
```

Example categories:

```env
# Application
APP_ENV=
APP_PORT=

# Database
DATABASE_URL=

# Authentication
JWT_SECRET=
JWT_EXPIRES_IN=

# Storage
STORAGE_PROVIDER=
STORAGE_BUCKET=
STORAGE_ACCESS_KEY=
STORAGE_SECRET_KEY=

# Email
MAIL_HOST=
MAIL_PORT=
MAIL_USER=
MAIL_PASSWORD=

# Maps / ETA
MAPS_API_KEY=
```

Only include variables actually used by the project.

---

# 16. Error Handling

The application must have a consistent error-handling strategy.

Errors should:

- Use appropriate HTTP status codes
- Return meaningful messages
- Avoid exposing sensitive implementation details
- Be logged appropriately
- Be understandable during debugging

Do not return raw database errors directly to users.

Example:

Bad:

```json
{
  "error": "PostgresError: relation users_xyz does not exist..."
}
```

Better:

```json
{
  "success": false,
  "error": {
    "code": "INTERNAL_SERVER_ERROR",
    "message": "Something went wrong. Please try again."
  }
}
```

---

# 17. Logging

The backend should implement useful application logging.

Logs should help developers identify:

- Authentication failures
- API errors
- Important state changes
- Trip lifecycle changes
- Driver assignments
- Payout processing
- Invoice verification
- Unexpected system errors

Do not log:

- Passwords
- Authentication secrets
- Tokens
- Private documents
- Sensitive personal information unnecessarily

---

# 18. Audit Logging

The PRD explicitly requires auditing for important changes.

At minimum, audit the following:

- Company payout split changes
- Trip amount changes
- Driver assignment/reassignment
- Invoice verification

The audit record should capture:

```text
Who
What
When
Which entity
Previous value (where applicable)
New value (where applicable)
```

Example:

```text
Admin: ADM-001
Action: DRIVER_REASSIGNED
Entity: TRIP-1024
Previous Driver: DRV-001
New Driver: DRV-005
Timestamp: 2026-09-30T12:00:00Z
```

---

# 19. Testing Guidelines

Testing should be performed throughout development rather than only at the end.

## 19.1 Backend Testing

At minimum, test critical business logic such as:

- Authentication
- Authorization
- Order creation
- Order acceptance/rejection
- Driver assignment
- Trip status transitions
- OTP validation
- Expense validation
- Payout calculation
- Wallet credit
- Duplicate payout prevention
- Invoice generation
- Two-person verification

## 19.2 Frontend Testing

Test important user workflows:

### B2B

```text
Login
→ Create Order
→ View Order
→ Track Trip
→ View Trip Report
→ View Verified Statement
```

### Admin

```text
Login
→ View Order
→ Accept Order
→ Assign Driver
→ Monitor Trip
→ Generate Statement
→ Verify Statement
→ Publish Statement
```

### Driver

```text
Login
→ Complete Onboarding
→ View Assigned Trip
→ Start Trip
→ Add Expenses
→ Enter OTP
→ Close Trip
→ View Wallet
```

## 19.3 Manual Testing

Teams must also perform end-to-end manual testing using realistic data.

---

# 20. Git and Version Control

Git must be used throughout development.

## 20.1 Commit Messages

Use meaningful commit messages.

Recommended format:

```text
feat: add driver assignment API
fix: prevent duplicate wallet credit
docs: update database documentation
refactor: simplify trip service
test: add payout calculation tests
```

Avoid:

```text
update
changes
final
final2
done
```

## 20.2 Branching

Teams should use a sensible branching strategy.

Example:

```text
main
develop
feature/*
fix/*
```

The exact strategy may differ, but the repository should remain organized.

## 20.3 Pull Requests

Where possible, use pull requests for meaningful changes.

PRs should explain:

- What changed
- Why it changed
- Testing performed
- Any known limitations

---

# 21. Documentation Structure

The root `docs/` directory must contain the project's technical documentation.

Recommended structure:

```text
docs/
│
├── architecture.md
├── api-documentation.md
├── database.md
├── er-diagram.md
├── naming-conventions.md
├── environment.md
├── installation.md
├── deployment.md
├── technical-decisions.md
├── security.md
├── testing.md
├── known-issues.md
└── workflows.md
```

Additional documents may be added when required.

---

# 22. Required Documentation

## 22.1 Project Setup Instructions

Document:

- Prerequisites
- Required runtime versions
- Required tools
- Repository setup
- Dependency installation
- Environment setup
- Database setup
- Seed data
- Running the application

## 22.2 Architecture Documentation

Document:

- Overall architecture
- Modular monolith structure
- Module responsibilities
- Application communication
- Authentication flow
- Important data flows
- External integrations
- Deployment architecture

Recommended file:

```text
docs/architecture.md
```

## 22.3 API Documentation

Document all implemented APIs.

Recommended file:

```text
docs/api-documentation.md
```

Swagger/OpenAPI can be included in addition to Markdown documentation.

## 22.4 Database Documentation

Document:

- Database technology
- Schema
- Tables
- Columns
- Relationships
- Constraints
- Indexes
- Important decisions
- Migration strategy

Recommended:

```text
docs/database.md
```

## 22.5 ER Diagram

The ER Diagram must be included.

Recommended:

```text
docs/er-diagram.md
docs/er-diagram.png
```

or an equivalent diagram format.

The diagram must represent the actual implemented database structure.

## 22.6 Environment Configuration

Document:

- Environment variables
- Development configuration
- Production configuration
- Required third-party services

Recommended:

```text
docs/environment.md
```

## 22.7 Installation Instructions

Document how a new developer can clone and run the complete project.

A reviewer should be able to follow the instructions without asking the team for basic setup information.

## 22.8 Deployment Instructions

Document:

- Build commands
- Deployment platform
- Environment configuration
- Database deployment/migrations
- Storage configuration
- Domain/API configuration
- Post-deployment verification

Recommended:

```text
docs/deployment.md
```

## 22.9 Important Technical Decisions

Document significant decisions such as:

```text
Why PostgreSQL?
Why Modular Monolith?
Why this authentication approach?
Why this ORM/query builder?
Why this file-storage solution?
Why this state-management approach?
Why this deployment architecture?
```

Each decision should explain:

```text
Decision
Reason
Alternatives considered
Trade-offs
```

Recommended:

```text
docs/technical-decisions.md
```

## 22.10 Known Issues

Document known bugs, limitations, assumptions, or incomplete functionality.

Do not hide known issues.

Recommended:

```text
docs/known-issues.md
```

## 22.11 Testing Information

Document:

- Testing strategy
- Test framework
- Unit tests
- Integration tests
- End-to-end tests
- How to run tests
- Current coverage where available
- Important manually tested workflows

Recommended:

```text
docs/testing.md
```

## 22.12 Security Considerations

Document:

- Authentication
- Authorization
- Password security
- Token security
- Input validation
- File upload security
- API security
- Database security
- Secrets management
- Access control
- Known security limitations

Recommended:

```text
docs/security.md
```

---

# 23. README.md Requirements

The root `README.md` must provide a concise overview of the entire project.

It should include:

```text
# HireD

## Overview

## Features

## Applications

- Admin Portal
- B2B Portal
- Driver App
- Backend

## Technology Stack

## Repository Structure

## Prerequisites

## Installation

## Environment Setup

## Running the Applications

## Database Setup

## Testing

## Deployment

## Documentation

## Team Members
```

The README should link to the detailed documents in `docs/`.

---

# 24. PRD Compliance

The implementation must follow the provided PRD.

The PRD includes the complete end-to-end workflow:

```text
B2B Order
    ↓
Admin Accept / Reject
    ↓
Trip
    ↓
Driver Assignment
    ↓
Trip Start
    ↓
Expenses
    ↓
OTP Trip Close
    ↓
Trip Report
    ↓
Driver Payout
    ↓
Month-End Statement
    ↓
Two-Person Verification
    ↓
B2B Partner Access
```

Teams should not implement isolated features without ensuring that the complete workflow works end to end.

---

# 25. Status and State Management

The application must follow the statuses defined in the PRD.

### Order

```text
Received
   ↓
Accepted / Rejected
```

### Trip

```text
Unassigned
   ↓
Assigned
   ↓
Ongoing
   ↓
Closed
```

### Invoice

```text
Draft
   ↓
Pending Verification
   ↓
Verified
   ↓
Published
```

### Payout

```text
Pending
   ↓
Credited
```

Invalid state transitions must be prevented by the application.

---

# 26. Financial Data Handling

The project contains financial information including:

- Trip amount
- Driver share
- Company share
- Wallet balance
- Payroll records
- Statements/invoices

Financial calculations must be deterministic and reproducible.

For each closed trip, store the applied payout rate and calculated amounts.

Example:

```text
Trip Amount: ₹10,000
Driver Rate: 30%
Company Rate: 70%

Driver Share: ₹3,000
Company Share: ₹7,000
```

Historical trips must not change when a company's payout rate is edited later.

---

# 27. Two-Person Invoice Verification

The PRD requires two-person verification.

The system must record:

```text
Statement Creator
First Approval
Second Approval
Approval timestamps
```

The same Admin must not be allowed to act as both required people.

The statement must not become `Published` until the required verification is completed.

---

# 28. API and Database Consistency

API contracts and database structure must remain synchronized.

When changing:

- Database schema
- API request
- API response
- Business rules
- Status values

the relevant documentation must also be updated.

Do not leave documentation describing an older implementation.

---

# 29. Deployment

The final project must have a documented deployment strategy.

The deployment documentation should clearly identify:

```text
Admin Portal
      ↓
Hosting

B2B Portal
      ↓
Hosting

Driver App
      ↓
Build / Distribution

Backend
      ↓
Server / Cloud

SQL Database
      ↓
Managed / Hosted Database

File Storage
      ↓
Secure Storage Provider
```

The exact cloud provider is not mandatory unless specified separately.

---

# 30. Final Technical Review Checklist

Before final submission, teams should verify:

## Architecture

- [ ] Backend follows Modular Monolithic Architecture
- [ ] Business modules are clearly separated
- [ ] Controllers are not overloaded with business logic
- [ ] Database access is properly organized

## Database

- [ ] SQL database is used
- [ ] Schema is normalized appropriately
- [ ] Primary keys are defined
- [ ] Foreign keys are defined
- [ ] Constraints are defined
- [ ] Indexes are considered
- [ ] ER Diagram is updated
- [ ] Database design was submitted within the first 2 days

## Applications

- [ ] Admin Portal works
- [ ] B2B Portal works
- [ ] Driver App works
- [ ] All applications communicate with the backend correctly
- [ ] UI follows the provided HireD theme
- [ ] Official logo is used

## Backend

- [ ] Authentication works
- [ ] Authorization works
- [ ] API validation exists
- [ ] Error handling is consistent
- [ ] API documentation is complete
- [ ] Critical business logic is tested

## Payroll

- [ ] Company payout split is stored
- [ ] Correct rate is applied
- [ ] Historical rate is preserved
- [ ] Duplicate wallet credits are prevented
- [ ] Wallet transaction is recorded

## Invoicing

- [ ] Monthly statement generation works
- [ ] Two-person verification works
- [ ] Verification timestamps are recorded
- [ ] Unverified statements are not published

## Security

- [ ] Passwords are securely hashed
- [ ] Secrets are not committed
- [ ] `.env.example` exists
- [ ] API access is protected
- [ ] File uploads are validated
- [ ] Partner data isolation works
- [ ] Driver data isolation works
- [ ] Sensitive information is not unnecessarily logged

## Documentation

- [ ] README completed
- [ ] Architecture documented
- [ ] API documented
- [ ] Database documented
- [ ] ER Diagram included
- [ ] Naming conventions documented
- [ ] Environment configuration documented
- [ ] Installation documented
- [ ] Deployment documented
- [ ] Technical decisions documented
- [ ] Known issues documented
- [ ] Testing documented
- [ ] Security documented
- [ ] Workflows documented

---

# 31. Final Repository Checklist

The final repository should approximately look like:

```text
project-root/
│
├── admin-portal/
│   ├── src/
│   ├── tests/
│   ├── .env.example
│   └── README.md
│
├── b2b-portal/
│   ├── src/
│   ├── tests/
│   ├── .env.example
│   └── README.md
│
├── driver-app/
│   ├── src/
│   ├── assets/
│   ├── tests/
│   ├── .env.example
│   └── README.md
│
├── backend/
│   ├── src/
│   │   ├── modules/
│   │   ├── shared/
│   │   └── config/
│   ├── tests/
│   ├── migrations/
│   ├── .env.example
│   └── README.md
│
├── docs/
│   ├── architecture.md
│   ├── api-documentation.md
│   ├── database.md
│   ├── er-diagram.md
│   ├── er-diagram.png
│   ├── naming-conventions.md
│   ├── environment.md
│   ├── installation.md
│   ├── deployment.md
│   ├── technical-decisions.md
│   ├── known-issues.md
│   ├── testing.md
│   ├── security.md
│   └── workflows.md
│
└── README.md
```

---

# 32. Core Principle

The Buildathon should be approached as a real-world software engineering project.

The expected engineering process is:

```text
Understand PRD
      ↓
Design Database
      ↓
Define Architecture
      ↓
Implement Backend
      ↓
Implement Web & Mobile Applications
      ↓
Integrate
      ↓
Test
      ↓
Secure
      ↓
Document
      ↓
Fix Bugs
      ↓
Deploy
      ↓
Demonstrate
```

The goal is not only to make the application work.

Teams must be able to explain:

- Why the architecture was chosen
- Why the database was designed that way
- How the modules interact
- How APIs are structured
- How authentication and authorization work
- How financial calculations are protected
- How duplicate operations are prevented
- How security is handled
- How the applications communicate
- How the project can be maintained and deployed

Every team member should understand the overall project and be able to explain the technical decisions related to their contribution.

---

## References

- HireD Phase 1 MVP Product Requirements Document
- Buildathon Guidelines & Criteria
- HireD Admin Portal design reference: https://id-preview--29526d21-889b-410d-affc-7d0297827ba6.lovable.app/
