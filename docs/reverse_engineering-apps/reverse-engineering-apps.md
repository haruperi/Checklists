You basically want to **turn someone else’s codebase into your own clean spec + design + plan**, then rebuild it. Love it.

Here’s a structured way to do it, step-by-step, with templates you can literally drop into:

---

## 0. Quick Legal / Licensing Check (Don’t skip)

1. **Check the repo license** (MIT, GPL, Apache, proprietary, none, etc.).
2. If it’s:

   * **MIT / Apache / BSD** → You can reuse ideas and even code with attribution (but you want your own implementation anyway).
   * **GPL** → Be careful: derivative work must also be GPL if distributed.
   * **No license** → Legally “all rights reserved” by default. Use only ideas, not code.
3. Your approach here:
   👉 **Study the design & behavior, then re-implement cleanly in your own codebase and docs.**

---

## 1. High-Level Recon: Understand the App as a User

Before touching the code in detail, you want a **product view**, not a **code view**.

### 1.1 Run the app

* Clone repo
* Follow their README
* Run it locally (or use their demo)

### 1.2 Map the main user journeys

Pretend you’re a product manager. List **flows**, not functions:

* User types:

  * e.g. *Admin, Normal User, Guest, API client*
* Main flows:

  * Sign up / login / logout
  * Create / Edit / Delete core entities (e.g. “strategy”, “project”, “task”)
  * Upload / download / export things
  * Dashboards / reports / analytics
  * Settings / permissions / notifications

**Output file:** `docs/01_FeatureInventory.md`

Example structure:

```md
# Feature Inventory

## 1. User Types
- Guest
- Registered User
- Admin

## 2. Main Features
2.1 Authentication & Accounts
- Register
- Login
- Logout
- Password reset

2.2 Core Domain: Strategies
- Create strategy
- Edit strategy
- Backtest strategy
- View performance metrics

2.3 Reporting
- Strategy performance dashboard
- Export results to CSV/Excel

...
```

This becomes the seed for your **System Requirements** later.


---

## 2. Technical Recon: Understand the Architecture

Now switch to **engineer mode**.

### 2.1 Codebase map

Scan the repo and write:

* Programming language(s)
* Frameworks (Django, FastAPI, React, etc.)
* High-level folder structure
* Where the “core logic” lives vs. “UI” vs. “infra”

**Output file:** `docs/02_CodebaseOverview.md`

Example:

```md
# Codebase Overview

## 1. Tech Stack
- Backend: Python 3.11, FastAPI
- Frontend: React (Next.js)
- DB: PostgreSQL
- Cache: Redis
- Message Queue: Celery + RabbitMQ

## 2. Folder Structure (Original)
- /backend/app
  - /models
  - /routers
  - /services
- /frontend
  - /components
  - /pages
- /infra
  - docker-compose.yml
  - nginx.conf
```

### 2.2 Identify modules and boundaries

From reading code + imports, map out **major modules**:

* Auth
* Users
* Core domain (e.g. Orders, Trades, Strategies)
* Analytics / Reports
* Integrations (external APIs)
* Background jobs

**Output file:** `docs/03_ArchitectureReverseEngineered.md`



---

## 3. From Reverse-Engineering → System Requirements (SRS.md)

Now you translate everything into a clean **System Requirements Specification** for *your* app.

### 3.1 Decide: Exact clone or “my improved version”?

* Mark which features are:

  * **Must have (MVP)**
  * **Nice to have**
  * **Out of scope**

### 3.2 SRS Skeleton

Create `docs/SRS.md` with something like:

```md
# System Requirements Specification (SRS)

## 1. Introduction
1.1 Purpose  
1.2 Scope  
1.3 Definitions, Acronyms, Abbreviations  
1.4 References  
1.5 Overview  

## 2. Overall Description
2.1 Product Perspective  
2.2 Product Functions  
2.3 User Classes and Characteristics  
2.4 Operating Environment  
2.5 Design and Implementation Constraints  
2.6 Assumptions and Dependencies  

## 3. Functional Requirements
3.1 Authentication & Authorization
  FR-1: The system SHALL allow users to register with email and password.
  FR-2: The system SHALL support JWT-based authentication.
  ...

3.2 Core Domain: [Your Domain]
  FR-X: ...
  
3.3 Reporting & Analytics  
3.4 Integrations  
3.5 Admin & Settings  



## 4. Non-Functional Requirements
4.1 Performance  
4.2 Security  
4.3 Reliability & Availability  
4.4 Scalability  
4.5 Usability  
4.6 Maintainability  
4.7 Logging & Monitoring  

## 5. Future Enhancements
```

You fill this by:

* Taking the **Feature Inventory** from step 1
* Rewriting each feature as a formal **FR-1, FR-2, …**

---

## 4. Design Document (Design.md)

Now you go from **what** to **how**.

Create `docs/Design.md` with these sections:

```md
# System Design

## 1. Architecture Overview
- Overall architecture style (monolith, microservices, modular monolith)
- High-level components

## 2. Component Design
2.1 Auth Service
  - Responsibilities
  - Interfaces / APIs
  - Dependencies

2.2 User Service
2.3 Core Domain Service(s)
2.4 Reporting Module
2.5 Frontend App
2.6 Background Workers

## 3. Data Design
3.1 Database Choice & Rationale
3.2 ERD (Entity-Relationship Diagram)
3.3 Key Tables / Collections
  - users
  - strategies
  - backtests
  - trades
  - etc.

## 4. API Design
4.1 API Principles
4.2 REST/GraphQL endpoints overview
4.3 Example endpoints

## 5. Security Design
- AuthN / AuthZ model
- Token lifetimes, password storage, etc.

## 6. Performance & Scalability
- Caching strategy
- Queues / background jobs
- Horizontal / vertical scaling options

## 7. Error Handling, Logging, Monitoring
```

As you read the original app’s code, you **document your own architecture decisions**, not just copy theirs.



---

## 5. Diagrams: Use Cases, Classes, Sequences

### 5.1 Use Case Diagrams

File: `docs/UseCaseDiagrams.md`

Example (Mermaid):

````md
```mermaid
graph TD
  User --> (Register)
  User --> (Login)
  User --> (Create Strategy)
  User --> (Run Backtest)
  User --> (View Performance Report)

  Admin --> (Manage Users)
  Admin --> (Configure System Settings)
````

````

You can also keep a textual version:

```md
## Use Cases

### UC-01: User registers an account
- Primary Actor: Guest
- Precondition: User is not logged in
- Main Flow:
  1. User opens the registration page
  2. User enters email, password
  3. System validates and creates account
  4. System sends confirmation email
- Postcondition: User account exists and is in pending verification state
````

---

### 5.2 Class Diagrams

File: `docs/ClassDiagram.MD` (text) + `docs/ClassDiagram.Mermaid`

Simple example (domain layer):

````md
```mermaid
classDiagram
  class User {
    +id: UUID
    +email: string
    +password_hash: string
    +created_at: datetime
  }

  class Strategy {
    +id: UUID
    +user_id: UUID
    +name: string
    +config: json
    +created_at: datetime
  }

  class Backtest {
    +id: UUID
    +strategy_id: UUID
    +start_date: date
    +end_date: date
    +status: string
    +result_summary: json
  }

  User "1" --> "many" Strategy
  Strategy "1" --> "many" Backtest
````

````

You derive this by looking at models/entities in the original code, then **cleaning them up** into your own ideal design.

---

### 5.3 Sequence Diagrams

File: `docs/SequenceDiagrams.MD`

Example: “Run a Backtest”

```md
```mermaid
sequenceDiagram
  actor User
  participant Frontend
  participant Backend
  participant Worker
  participant DB

  User ->> Frontend: Click "Run Backtest"
  Frontend ->> Backend: POST /backtests
  Backend ->> DB: Insert backtest record (status="queued")
  Backend ->> Worker: Enqueue job
  Worker ->> DB: Fetch backtest + strategy config
  Worker ->> Worker: Run backtest engine
  Worker ->> DB: Save results, update status="completed"
  User ->> Frontend: View results page
  Frontend ->> Backend: GET /backtests/{id}
  Backend ->> DB: Fetch backtest
  Backend ->> Frontend: Return results (equity curve, stats)
````

````





---

## 6. Implementation Plan (ImplementationPlan.md)

This is your **roadmap from zero → finished app**.

File: `docs/ImplementationPlan.md`

### 6.1 Phases

Proposed phases:

1. **Phase 0 – Foundations**
   - Setup repo, basic folder structure
   - Choose stack & tools
   - Setup CI, formatter, linter, test framework

2. **Phase 1 – Core Infrastructure**
   - Auth & users
   - Base models & DB schema
   - Generic API layer

3. **Phase 2 – Core Domain**
   - Implement main entities (strategies / orders / tasks / etc.)
   - CRUD APIs
   - Business rules

4. **Phase 3 – Background Processing / Engine**
   - Job queue setup
   - Core processing engine (e.g. backtesting engine)

5. **Phase 4 – UI / Dashboard**
   - Main screens matching your use cases
   - Charts, tables, filters

6. **Phase 5 – Reporting / Exports**
7. **Phase 6 – Hardening**
   - Security, logging, error handling, docs, tests

### 6.2 Turn into actionable checklist

Example snippet:

```md
# Implementation Plan Checklist

## Phase 0 – Foundations
- [ ] Initialize git repo: `haruapp/`
- [ ] Setup Python backend (FastAPI) project
- [ ] Setup Node/React frontend project
- [ ] Add code formatter (Black / Prettier)
- [ ] Add linter (Flake8 / ESLint)
- [ ] Setup basic CI (GitHub Actions)
- [ ] Create `AGENTS.md` with coding guidelines

## Phase 1 – Auth & Users
- [ ] Define User model in DB
- [ ] Implement registration endpoint
- [ ] Implement login endpoint (JWT)
- [ ] Implement password reset flow
- [ ] Add tests for auth APIs
- [ ] Implement auth in frontend (login form, protected routes)

## Phase 2 – Core Domain
- [ ] Define core entities (Strategy, Backtest, Result, etc.)
- [ ] Implement CRUD APIs
- [ ] Add domain logic validation
- [ ] Tests for domain services

...
````



---

## 7. Putting It All Together – Suggested Docs Folder

```text
/docs
  SRS.md
  Design.md
  UseCaseDiagrams.md
  ClassDiagram.MD
  ClassDiagram.Mermaid
  SequenceDiagrams.MD
  ImplementationPlan.md
  01_FeatureInventory.md
  02_CodebaseOverview.md
  03_ArchitectureReverseEngineered.md
```




---


