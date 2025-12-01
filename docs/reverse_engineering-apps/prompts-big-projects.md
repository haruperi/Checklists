## Prompt 1

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



## Prompt 2

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

## Prompt 3

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


## Prompt 4

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


## Prompt 5

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

## Prompt 6

```md
Using the SRS, Design, and Diagrams documents, produce a detailed complete **ImplementationPlan.md**.

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