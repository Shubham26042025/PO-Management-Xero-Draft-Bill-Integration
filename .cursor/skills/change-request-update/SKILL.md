---
name: change-request-update
description: Update existing BRD, FRD, and UAC documents by incorporating Change Requests. Use when applying change requests, updating requirements, or versioning documents with new business rules.
---

# Change Request Update

Update existing BRD, FRD, and UAC documents by incorporating Change Requests. **DO NOT create new standalone documents.** Update existing documents in-place with version history.

## Core Principle

- **Update existing documents**, do not create new ones
- **Preserve version history** - add new version entries, don't remove old ones
- **Update in-place** - modify impacted sections, keep unaffected sections unchanged
- **Maintain consistency** - ensure BRD ↔ FRD ↔ UAC remain aligned

## Phase 1: Change Request Analysis

### Understand the Change Request

Extract from the change request:

1. **Old Rule** - What needs to be removed/changed
2. **New Rule** - What replaces it
3. **Reason** - Business justification
4. **Impact Scope** - Which modules/features are affected

### Identify Impacted Sections

For each document (BRD, FRD, UAC), identify:

- Sections mentioning the old rule
- Business rules sections
- Functional requirements sections
- User stories/acceptance criteria sections
- Data model sections (if applicable)
- Validation rules sections

## Phase 2: Document Location & Reading

### Locate Existing Documents

1. **BRD**: `BA Docs/Business Requirement Document/[BRD filename]`
2. **FRD**: `BA Docs/Functional Requirement Document/[FRD filename]`
3. **UAC**: `BA Docs/User Stories and Acceptance Criteria/[UAC filename]`

### Read All Documents

- Read the complete BRD to understand current structure
- Read the complete FRD to understand functional requirements
- Read the complete UAC CSV to understand acceptance criteria
- Identify all occurrences of the old rule across all documents

## Phase 3: BRD Update Process

### Step 1: Update Version History

Add new version row to version history table (do not remove existing rows):

```
| Version | Approved By | Revision Date | Description Of Change | Author |
| ----- | ----- | ----- | ----- | ----- |
| [existing rows...] |
| [next version] | [TBD] | [current date] | Multi-company user access (max 2 companies) with role-per-company model | [BA/Product Analyst] |
```

### Step 2: Add Change Request Subsection

In relevant section (e.g., "Requirement Discussed" or new "Change Requests" section):

```
## Change Request: [CR Number/Title]

**Date**: [Date]
**Requested By**: [Stakeholder]
**Reason**: [Business justification]

**Old Rule**: [Old rule description]
**New Rule**: [New rule description]

**Impact**: [List impacted modules/features]
```

### Step 3: Update Business Requirements

**Remove** all instances of old rule:
- Search for phrases like "one company only", "single company", "cannot access other company"
- Remove or update these statements

**Add** new business requirement:
- One user can be linked to up to 2 companies (maximum)
- User role is company-specific (not global)
- Access and permissions are determined by selected company context
- Same user can have different roles in different companies

### Step 4: Update Stakeholder/Business Need

Add explanation:
- Business need: Shared users across related companies
- Benefit: Reduced duplicate accounts and better governance
- Control: Per-company role management

### Step 5: Update Scope Impact

Mark impacted modules:
- Company Management
- User Management
- Role Management
- Login/Authentication
- Company Selector/Switching
- Access Control/Permissions

### Step 6: Update Business Rules Section

**Remove**: One-company-only restrictions

**Add** updated business rules:
- Maximum 2 companies per user
- Role assigned per company (not globally)
- No duplicate user-company mapping allowed
- Global user disable overrides company-level access
- Company-level disable affects only that company's access
- Access evaluated in company context

### Step 7: Update Success Criteria

Add/update outcomes:
- Single user can access up to 2 allowed companies
- Correct role behavior per company context
- No unauthorized cross-company access
- Backward compatible login for single-company users

## Phase 4: FRD Update Process

### Step 1: Update Version History

Add new version entry (preserve existing history).

### Step 2: Add Change Request Summary

```
## Change Request Summary

**Old Rule**: [Old rule]
**New Rule**: [New rule]
**Reason**: [Business justification]
**Impact**: [List impacted modules/screens]
```

### Step 3: Update Business Rules Section

**Remove**: All references to one-company-only restrictions

**Add**:
- User can belong to multiple companies (maximum 2)
- Role is per-company (not global)
- Access scope is company-context based
- Role differences across companies remain isolated

### Step 4: Update Functional Scope

Update impacted modules:
- Company Management
- User Management
- Role Management
- Login & Authentication
- Company Switching/Selector
- Audit Logs
- Access Control

### Step 5: Update Functional Flows

Add/modify flows for:

**Add Existing User to Another Company**:
- Flow: User Management → Select User → Add Company Access → Assign Role → Save
- Validation: Check if user already has 2 companies, prevent if max reached

**Login Behavior**:
- User in 1 company → Direct login to that company
- User in 2 companies → Show company selector OR auto-select last used company (per config)
- Store last selected company preference

**Company Switching** (if enabled):
- Flow: Header → Company Selector → Switch → Reload with new company context
- Permission re-evaluation on switch

### Step 6: Update Screen/UX Requirements

**User Create/Edit Screen**:
- Add "Company Access" section
- Multiple company rows (maximum 2 rows)
- Role dropdown per company row
- Company-level status (Active/Inactive) per row
- Remove single company restriction UI elements

**Login Screen**:
- Company selector dropdown (for multi-company users)
- Auto-select last used company option (if configured)

**Header/App**:
- Company switcher component (if applicable)
- Display current company context

### Step 7: Update Validation Rules

**Remove**: One-company-only validations

**Add**:
- Same user/email can be mapped to up to 2 companies
- In one company, user must have exactly one active role
- Prevent duplicate mapping (same user + same company)
- Remove from one company → lose access only for that company
- Global disable user → lose access across all companies
- Do not allow >2 companies per user
- Role update in Company A does not impact Company B

### Step 8: Update Permissions Model

**Remove**: Global role assumptions

**Add**:
- Permission check = (selected company) + (role in selected company)
- Role differences across companies remain isolated
- Company context determines available permissions
- Switching company re-evaluates permissions

### Step 9: Update Data Model

**Add/Update** mapping table structure:

```
UserCompanyRole (
  user_id,
  company_id,
  role_id,
  status (Active/Inactive),
  created_at,
  updated_at,
  created_by,
  updated_by,
  deleted_at (nullable)
)
```

**Constraints**:
- Unique active mapping on (user_id, company_id)
- Maximum active companies per user = 2
- Soft delete supported
- Index on user_id for fast lookup

### Step 10: Update Migration/Backward Compatibility

- Existing single-company users migrate to one mapping row
- No login break for existing users
- Legacy role references aligned to mapping model
- Migration script creates UserCompanyRole entries from existing data

### Step 11: Update Out of Scope

Clearly state (unless separately requested):
- SSO changes
- Multi-role per company
- Bulk import
- Approval workflow
- More than 2 companies per user

### Step 12: Consistency Check

**Remove all traces** of "one user only one company" language from:
- Every section
- Tables
- Rules
- Flows
- UI notes
- Validation rules

## Phase 5: UAC Update Process

### Step 1: Read Existing UAC CSV

Parse existing UAC CSV file, identify:
- Current UAC rows
- Modules covered
- Last UAC number used

### Step 2: Generate New UAC Rows

**Minimum 15 new UAC rows** for the change request.

**Strict CSV Format** (exact columns):
```
No | Module | AS A/AN | I want to | So That | # | Questions | Acceptance Criteria
```

### Step 3: UAC Coverage Requirements

Cover these scenarios:

**Positive Scenarios**:
- Add user to second company with role
- Login with 2 companies (company selector appears)
- Switch company post-login
- User has different roles in different companies
- Access permissions work correctly per company

**Negative Scenarios**:
- Attempt to add user to third company (blocked)
- Attempt duplicate mapping (blocked)
- Invalid role assignment per company
- Access denied when company context doesn't match

**Boundary Cases**:
- Maximum 2 companies per user (enforced)
- User with exactly 2 companies (all flows work)
- User with 1 company (backward compatible)

**Role-Per-Company**:
- Admin in Company A, Sales in Company B
- Role change in Company A doesn't affect Company B
- Permission isolation between companies

**Login Behavior**:
- Single company → direct login
- Two companies → company selector or auto-select
- Last used company remembered

**Disable Scenarios**:
- Company-level disable → lose access only to that company
- Global disable → lose access to all companies
- Company deactivated → mapped users lose access to that company only

**Validation/Error Messages**:
- Clear error when trying to add third company
- Clear error for duplicate mapping
- Success messages for valid operations

### Step 4: Group UAC Rows by Intent

Group rows with same:
- AS A/AN
- I want to
- So That

Then vary:
- Questions (#)
- Acceptance Criteria

### Step 5: Append to Existing UAC

**Do not replace** existing UAC rows.

**Append** new UAC rows to the existing CSV file.

**Numbering**: Continue from last UAC number in existing file.

**Module Separation**: Insert blank row after finishing each module group.

### Step 6: Add Post-UAC Section

After UAC table, add:

```
## Missing / Unclear Requirements
[List any unclear requirements]

## Assumptions Made
[List assumptions]

## Client Questions to Confirm

### Access Model
- [Question 1]
- [Question 2]

### UX Behavior
- [Question 1]
- [Question 2]

### Security/Compliance
- [Question 1]
- [Question 2]

### Migration
- [Question 1]
- [Question 2]
```

## Phase 6: Consistency Validation

### Cross-Document Consistency Check

Verify:

1. **BRD ↔ FRD**:
   - Business rules match functional requirements
   - Scope alignment
   - Business need reflected in functional flows

2. **FRD ↔ UAC**:
   - Functional requirements have corresponding UAC rows
   - Validation rules match acceptance criteria
   - Edge cases covered in both

3. **BRD ↔ UAC**:
   - Business rules testable via UAC
   - Success criteria measurable

### Remove Old Rule References

Search all documents for:
- "one company only"
- "single company"
- "cannot access other company"
- "only one company"
- Any variations of the old rule

Replace or remove these references.

## Phase 7: Document Update Execution

### Update Strategy

1. **Read** existing document completely
2. **Identify** all sections to update
3. **Update in-place**:
   - Modify impacted paragraphs
   - Add new subsections where needed
   - Remove old rule references
   - Add version history entry
4. **Preserve**:
   - Unaffected sections (keep as-is)
   - Existing version history
   - Document structure
   - Formatting style

### Version History Pattern

Always add (never replace):
- New version row
- Current date
- Change summary
- Author name

### Change Request Subsection Pattern

Add to relevant section:
- Change request title/number
- Date
- Old rule → New rule
- Reason
- Impact summary

## Phase 8: Quality Checks

Before finalizing:

- [ ] Version history updated in all documents
- [ ] Old rule removed from all documents
- [ ] New rule added consistently
- [ ] Impacted modules identified
- [ ] Functional flows updated
- [ ] UAC rows added (minimum 15)
- [ ] Edge cases covered
- [ ] Consistency across BRD/FRD/UAC
- [ ] No orphaned references to old rule
- [ ] Client questions documented
- [ ] Assumptions listed

## Example: Change Request Update Flow

```
1. Read: BRD, FRD, UAC documents
2. Analyze: Old rule "one company only" → New rule "max 2 companies"
3. Update BRD:
   - Add version history entry
   - Add Change Request subsection
   - Update Business Requirements
   - Update Business Rules
   - Update Scope Impact
4. Update FRD:
   - Add version history entry
   - Add Change Request Summary
   - Update Business Rules
   - Update Functional Flows
   - Update Screen Requirements
   - Update Validation Rules
   - Update Data Model
5. Update UAC:
   - Generate 15+ new UAC rows
   - Append to existing CSV
   - Add Missing/Assumptions/Questions section
6. Validate: Consistency across all documents
7. Save: Updated documents (same filenames)
```

## Important Rules

1. **Never create new documents** - always update existing ones
2. **Never remove version history** - only add new entries
3. **Never rewrite from scratch** - update in-place only
4. **Always maintain consistency** - BRD ↔ FRD ↔ UAC must align
5. **Always preserve unaffected sections** - only update what's impacted
6. **Always document assumptions** - list in UAC post-section
7. **Always ask client questions** - document in UAC post-section

## Output Format

Return updated sections:

1. **BRD Updates**: Only impacted sections + version history
2. **FRD Updates**: Only impacted sections + version history  
3. **UAC Additions**: New UAC rows in strict CSV format + post-section
4. **Consistency Summary**: Brief note on cross-document alignment
