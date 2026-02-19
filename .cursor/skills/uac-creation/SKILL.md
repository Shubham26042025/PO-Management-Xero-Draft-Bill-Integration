---
name: uac-creation
description: Create User Acceptance Criteria in strict CSV format. Use when writing UAC, acceptance criteria, or generating test scenarios for a module.
---

# UAC Creation

## Pre-requisites (Mandatory)

Before writing UAC, confirm:

- Module name
- BRD approval
- FRD approval
- Mockups/UI references
- Sprint/release
- Exclusions/deferred scenarios

Do not generate UAC until module and requirements are clear.

## Output Format (Strict CSV)

Use EXACT column headers:

```
No | Module | AS A/AN | I want to | So That | # | Questions | Acceptance Criteria
```

- Insert ONE blank row after each module (empty CSV line).
- One intent (AS A/AN + I want to + So That) followed by multiple Questions and Acceptance Criteria rows.
- Do not change intent mid-group.

## Coverage Rules

- More than 15 UAC rows per module.
- Cover: positive, negative, edge/boundary, role-based, validations, permissions, dependency/integration failures.

## Acceptance Criteria Writing Rules

- Clear, testable, atomic, verifiable.
- If outcome is binary: respond ONLY with **Yes** or **No** (no extra text).
- Business-readable English; avoid "should work correctly" or similar vague phrases.

## Example CSV Row

```
1 | Purchase Order | As a Purchaser | I want to create a PO | So that I can request goods | 1 | Can PO be edited after creation? | Yes
```

## Delivery Notes

- CSV has no borders; user can apply borders in Excel using Ctrl+A → Borders.
