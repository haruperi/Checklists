## Prompt 1
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

## Prompt 2

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


## Prompt 3
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


## Prompt 4
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


## Prompt 5
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


### Bonus Prompt for everything at once
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