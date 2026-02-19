---
name: frd-creation
description: Create enterprise-grade Functional Requirements Documents for sprint-specific deliverables. Use when creating FRDs, drafting functional requirements, or analyzing BRD-to-FRD conversion for a sprint.
---

# FRD Creation

## Quick Start

1. Run Discovery & Inputs Collection checklist.
2. Analyze provided documents for implicit requirements and gaps.
3. Map sprint deliverables to BRD requirements and mockups.
4. Generate FRD using exact template structure.
5. Validate with open questions and assumptions list.

## Phase 1: Discovery & Inputs Collection

Before generating an FRD, ask for:

1. Approved **BRD** (upload or paste)
2. **Statement of Work (SOW)**
3. **Minutes of Meeting (MoM)** from discovery or sprint discussions
4. **Mockups, wireframes, or screenshots**
5. **Sprint Plan or Sprint Scope** document
6. **Sprint number** for this FRD
7. **Out-of-scope items** or explicitly deferred features for this sprint
8. **Known constraints** (budget, timeline, third-party dependency, compliance)
9. **Client expectations** not clearly written in the BRD

**Do not generate an FRD until sufficient clarity is achieved.** If any critical input is missing, pause and ask follow-up questions.

## Phase 2: Analysis Expectations

When documents are provided:

- Interpret **implicit requirements**, not just stated ones
- Identify **gaps between BRD, sprint scope, and mockups**
- Detect **unclear flows, missing validations, and undefined edge cases**
- Flag **non-functional expectations** (performance, auditability, security, scalability, usability)
- Identify **assumptions being made by the client or BA**
- Note **dependencies on third-party systems, APIs, or data availability**

Clearly list **questions that should be asked to the client** before final sign-off.

## Phase 3: Sprint Mapping Logic

- Parse the sprint plan to identify sprint deliverables
- Map each deliverable to: BRD requirements, mockups/UI references, business workflows
- Clearly separate: what is included in this sprint vs what is future scope
- **Do not mix multiple sprints in a single FRD**

## Phase 4: FRD Generation Rules

- Generate the FRD using the **exact structure and order** defined in the provided FRD template
- Do NOT rename sections, reorder headers, or skip sections
- Only populate content based on validated inputs

## Writing Style & Tone

- Write in **clear, professional, human English**
- Assume the reader understands business but not internal logic
- Avoid robotic phrases like "system shall" unless necessary
- Explain **intent, usage, and impact**, not just functionality
- Prefer paragraphs over bullet points
- Use bullets only when they improve clarity

## Module-Level Writing

For each module or feature:

1. Begin with the **purpose of the module**
2. Explain **how users will interact with it in real scenarios**
3. Clarify: user roles involved, access boundaries, data ownership
4. Describe expected system behavior, validations, and outcomes
5. Highlight assumptions and constraints inline where relevant

If UI screens are provided: align explanations with **actual user behavior**, not just screen elements. Reference mockups or screenshots contextually.

## Mandatory Output Sections

Ensure the FRD includes:

- Project metadata
- Version history
- Authorization memorandum
- Introduction and purpose
- Business requirements overview
- Detailed functional requirements with user impact
- Assumptions and constraints

## Final Validation

Before finalizing:

- Summarize **open questions**
- List **identified gaps or risks**
- Clearly state any **assumptions made due to missing information**

Do not silently assume missing details.

## Document Storage

Save all FRD documents to: `BA Docs/Functional Requirement Document/`

Create the folder structure if it doesn't exist. Use descriptive file names (e.g., `FRD_PO_Creation_Sprint1.md`).
