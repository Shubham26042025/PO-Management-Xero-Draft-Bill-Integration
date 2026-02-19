---
name: brd-creation
description: Create Business Requirements Documents from meeting transcripts using the Satva template. Use when creating BRDs, drafting requirements, or extracting requirements from meeting transcripts.
---

# BRD Creation

## Quick Start

1. Locate the meeting transcript (typically in "Meeting Recording" folder).
2. Run the Transcript Extraction Checklist.
3. Use the Satva BRD template structure.
4. Mark "Client Confirmed" vs "Assumed/To Confirm" clearly.
5. End with Open Questions and Next Steps.

## Transcript Extraction Checklist

Before writing the BRD, extract from the transcript:

1. **Confirmed Requirements** – Client-approved statements
2. **Scope** – In Scope vs Out of Scope confirmations
3. **Business Rules** – Limits, constraints, approvals, roles
4. **Integrations** – What, when, direction of data sync
5. **Reporting** – What reports, frequency, audiences
6. **Non-functional** – Audit logs, security, performance, error visibility
7. **Dependencies** – Client inputs, third parties, API access
8. **Timelines** – Sprints, milestones
9. **Open Questions** – Unclear items

## BRD Structure (Satva Template)

```
1. Acknowledgment
2. Requirement Discussed
3. Solution Suggested
4. Development Flow
5. Assumptions/Constraints
6. Next Steps
```

Each section must start with a short paragraph (2–4 sentences). No bullet-only sections.

## Writing Rules

- Use professional, client-facing language.
- Keep statements testable and specific (avoid "should", "etc.", "as needed").
- If transcript and other notes conflict, transcript wins. Flag conflicts under Open Questions/Risks.
- Ask discovery questions only for gaps after transcript extraction. Do not assume missing requirements.

## Quality Gate

Before finalizing, ensure the BRD includes:

- Scope boundaries
- Key workflows
- Integrations
- Assumptions/constraints
- Next actions

**If transcript is missing, do not proceed.** Request transcript or key decisions.

## Document Storage

Save all BRD documents to: `BA Docs/Business Requirement Document/`

Create the folder structure if it doesn't exist. Use descriptive file names (e.g., `BRD_PO_Management_Sprint1.md`).
