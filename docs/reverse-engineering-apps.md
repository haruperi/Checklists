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

## 2. Prompt

```md
I want you to reverse-engineer this repository: <INSERT_REPO_URL>

Your task: Create a detailed reverse-engineered **Feature Inventory**.

Instructions:
- Do NOT describe the code.
- Describe the PRODUCT features and CAPABILITIES that the repo implements.
- Think like a Product Manager, not a developer.
- Group features logically (e.g. “Data Feeds”, “Execution Engine”, “Broker Handling”, “Strategy API”, “Analytics”, “Plotting”, etc.).
- For each feature:
    - Provide a short description
    - Identify users who benefit (e.g. Quant, Trader, Developer)
    - Describe dependencies if any
- Identify implicit features (e.g. scheduling, caching, message passing, etc.)
- Identify missing but expected features
- Output format: Markdown, file name = **01_FeatureInventory.md**
- Structure:
    1. Overview  
    2. Feature Groups  
    3. Detailed Feature List  
    4. Assumptions & Constraints  
    5. Notes for future SRS  

Do not write anything about implementation. Only the product features.
```

## Prompt for small projects 
```md
I want you to reverse-engineer this small repository: <INSERT_REPO_URL>

Produce a short, clear **Feature Inventory**.

Guidelines:
- List only core features (3–10 items)
- Explain each in 1–2 sentences
- Describe what the tool does, not how it works
- No architecture yet

Output file: FeatureInventory.md  
Structure:
1. Summary (3–5 sentences)  
2. Core Features (bullet list)  
3. Inputs / Outputs  
4. Limitations or missing features

```

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

## 3. Prompt

```md
Using the same repository (<INSERT_REPO_URL>), produce a clean, high-level **Reverse-Engineered Architecture Document**.

Instructions:
- Think like a senior software architect.
- Identify the major subsystems (e.g. Engine, Data Feeds, Broker, Indicators, Strategy API).
- For each subsystem, describe:
    - Responsibilities
    - Inputs / Outputs
    - How it collaborates with other components
- Identify architectural patterns used (event-driven, modular monolith, plugin system, observer pattern, etc.)
- Produce a conceptual architecture diagram (text + Mermaid)
- Outline the lifecycle flow of the system (initialization → run loop → shutdown)
- Describe key abstractions and their boundaries
- Identify weaknesses or architectural risks

Output file: **02_ArchitectureReverseEngineered.md**

Structure:
1. Introduction  
2. High-Level Architecture Summary  
3. Subsystem Breakdown  
4. Data Flow Explanation  
5. Control Flow / System Lifecycle  
6. Architecture Diagram (Mermaid)  
7. Architectural Strengths / Weaknesses  
8. Implications for our own redesign
```

## Prompt for small projects

```md
Using the small repository: <INSERT_REPO_URL>

Produce a simple **Architecture Overview**.

Guidelines:
- No deep breakdown, just the essentials
- Identify main components (files/modules) and their responsibilities
- Show how they connect
- Include 1 small Mermaid diagram

Output file: ArchitectureOverview.md  
Sections:
1. High-Level Description  
2. Component Responsibilities  
3. Data Flow (simple)  
4. Mermaid diagram

```

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

## 4. Prompt

```md
Using the Feature Inventory and Architecture documents, generate a full **Software Requirements Specification (SRS)** document for *my own version* of this system.

Important:
- Do NOT describe the original repo.
- Write an improved, clearer, cleaner SRS for MY system.
- I want a professional SRS document.

Structure (IEEE-style):
1. Introduction  
    1.1 Purpose  
    1.2 Scope  
    1.3 Definitions  
    1.4 References  
2. Overall Description  
    2.1 Product Perspective  
    2.2 Product Functions  
    2.3 User Classes  
    2.4 Operating Environment  
    2.5 Constraints  
    2.6 Assumptions  
3. Functional Requirements  
    (FR-1, FR-2, FR-3… grouped by domain)  
4. Non-functional Requirements  
5. Future Enhancements

Output file: **SRS.md**
Use clear, professional formatting.
```

## Prompt for small projects
```md
Using the Feature Inventory and Architecture Overview, generate a **Mini SRS**.

Constraints:
- Keep it short (1–2 pages max)
- Only include top-level requirements
- No deep classifications or IEEE-format sections

Output file: SRS.md  
Structure:
1. Purpose  
2. Scope  
3. Functional Requirements (5–12 items max)  
4. Non-Functional Requirements (3–6 items)  
5. Assumptions  
6. Future Enhancements
```

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

## 4. Prompt

```md
Using the SRS.md, generate a complete **System Design Document (Design.md)** for my system.

Rules:
- This is a technical design, not requirements.
- Think like a senior architect designing a brand-new system.
- No references to the original repo.
- Provide:
    - Architectural decisions
    - Class responsibilities
    - Subsystem designs
    - API design choices
    - Data model design
    - Execution loop model
    - Diagrams (architecture, flow, modules)

Structure:
1. Architecture Overview  
2. Component Design  
3. Data Design  
4. API Design  
5. Error Handling  
6. Security Model  
7. Performance & Scalability  
8. Logging & Monitoring  
9. Extensibility / Plugin Architecture  
10. Diagrams (Mermaid)

Output file: **Design.md**
```

## Prompt for small projects
```md
Generate a compact **Design Document** for this small project, based on the SRS.

Rules:
- The design must be simple and directly map to the existing files
- Summaries only
- 1–2 diagrams max
- No overly formal language

Output file: Design.md  
Sections:
1. Overview  
2. Components and Responsibilities  
3. Data Structures  
4. API or Function Overview  
5. One Mermaid diagram

```

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

## Prompt

```md
Generate the Diagrams package for my system using the SRS and Design documents.

I want all of the following:

1. **UseCaseDiagram.md**
    - UML-style textual description for each use case
    - Mermaid use case diagram

2. **ClassDiagram.MD**
    - Textual UML class diagram
    - Describe each class, attributes, methods, relationships

3. **ClassDiagram.Mermaid**
    - Mermaid class diagram only

4. **SequenceDiagram.MD**
    - Textual UML sequence diagrams for:
        - Running a backtest
        - Handling an order
        - Executing a strategy tick
    - Mermaid sequence diagrams

Instructions:
- Be clean, consistent, and professional.
- Base everything on the SRS + Design documents.
- This is for MY system, not the original repo.

Output 4 files:
- UseCaseDiagram.md
- ClassDiagram.MD
- ClassDiagram.Mermaid
- SequenceDiagram.MD
```


## Prompt for small projects

```md
Generate minimal diagrams for this small project.

Deliver:
1. Use Case Diagram (Mermaid + 2–3 use cases)
2. Class Diagram (if OOP) OR Module Diagram (if scripts)
3. One Sequence Diagram for the primary execution path

Output:
- UseCaseDiagram.md
- ClassOrModuleDiagram.md
- SequenceDiagram.md

Keep diagrams very small and simple.

```
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

```md
Using the SRS, Design, and Diagrams documents, produce a complete **ImplementationPlan.md**.

Rules:
- Break everything into concrete tasks.
- Use phases (Phase 0, Phase 1, Phase 2…)
- Each phase must have several related actionable tasks.
- Tasks must be detailed enough that a junior developer can execute them.
- Format:
    - Checkbox TODO list
    - Phases with explanations
    - Task to implement with its implementing steps.
    - Create and run unit tests
    - Create and run usage examples
    - Run an auto-formatter (e.g black)
    - Run lint + quality (e.g flake8)
    - User Documentation + Developer notes and warnings
    - Git Commit
   

Output file: **ImplementationPlan.md**
```

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

### Prompt
```md
I want to create a full documentation suite for reverse engineering this repository:

<INSERT_REPO_URL>

Please generate the following files, in order:

1. 01_FeatureInventory.md  
2. 02_ArchitectureReverseEngineered.md  
3. SRS.md  
4. Design.md  
5. UseCaseDiagram.md  
6. ClassDiagram.MD  
7. ClassDiagram.Mermaid  
8. SequenceDiagram.MD  
9. ImplementationPlan.md

Each file should be standalone and self-contained.
Each should build on the previous files.
This is for a system I will build myself — do NOT reproduce any code from the repo.
This must be a professional software engineering documentation set.


```


---


