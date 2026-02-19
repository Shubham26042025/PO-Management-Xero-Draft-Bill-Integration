# **Business Requirement Document**

| Name of The Project: | PO Management + Xero Draft Bill Integration |
| :---- | :---- |
| **For** DSB | **Prepared By:** Sonal (Lead BA), Satva Solutions |
| **Created On:** February 19, 2026 | |

| Amendment History |  |  |  |
| :---: | :---: | :---: | :---: |
| **Version** | **Prepared By** | **Date** | **Description** |
| 1.0 | Sonal (Lead BA) | 19/02/2026 | Initial BRD based on discovery call with Libbie and William |
|  |  |  |  |

---

## 1. Acknowledgment

Dear DSB Team (Libbie and William),

Thank you for considering Satva Solutions for your business requirements for the PO Management + Xero Draft Bill Integration project. This document captures the requirements discussed during our discovery call held on February 19, 2026 (~58 minutes, virtual). We appreciate the clarity and detail provided during the session and look forward to delivering a solution that eliminates duplicate work between your purchasing process and Xero accounting.

This BRD serves as the baseline agreement for scope, workflows, and integration expectations. It is structured as a client-facing deliverable for review, feedback, and written approval before development begins.

---

## 2. Requirement Discussed

DSB's core business need is to eliminate the duplicate manual data entry that occurs between their internal purchase order process and Xero accounting software. Currently, DSB prepares purchase orders internally, and then the accounting team manually re-enters the same data into Xero to create bills. This redundancy increases the risk of data entry errors, slows down the accounts payable cycle, and consumes time that could be directed toward higher-value financial activities.

### 2.1 Current Pain Point (Client Confirmed)

- Purchase orders are created internally by the DSB team.
- The accounting team then manually re-creates the same information in Xero as a bill.
- Xero is the source of truth for all accounting records. *(William confirmed: "Correct.")*

### 2.2 Core Functional Requirements (Client Confirmed)

The following requirements were explicitly confirmed by DSB during the discovery call:

**PO Creation:**

- The system must allow users to create purchase orders with the following header fields: Organization, Department, Supplier, Date, Notes, and Attachments. *(Libbie, 26:45)*
- Each PO must support multiple line items with the fields: Description, Quantity, Unit Price, Line Total, and GL Account. *(Libbie, 26:45)*
- Users must be able to save a PO in draft status without pushing to Xero. *(William: "Draft save allowed.")*

**Push to Xero:**

- Pushing a PO must create a draft bill in Xero. The bill must never be auto-approved. *(William: "Yes — as draft bill only." / Libbie: "Never auto approve.")*
- The system must store the Xero-returned reference ID against the PO record upon successful push. *(Libbie: "Save reference ID.")*
- The system must block the push operation if any required data is missing or incomplete. *(William: "Block if data missing.")*
- The Xero integration must be active and validated before any push operation is permitted. *(Sonal: "Integration must be active before push." / William: "Yes.")*

**Finance Workflow:**

- Finance team members finalize and approve bills exclusively inside Xero. The PO system does not participate in the approval workflow. *(Libbie: "Finance finalizes inside Xero only.")*

### 2.3 Integration Requirements (Client Confirmed)

| Sync Entity | Direction | Source | Trigger | Details |
| :--- | :---: | :---: | :---: | :--- |
| GL Accounts | Xero → PO System | Xero | Manual | GL accounts are pulled from Xero. Only synced accounts are available for selection on PO line items. *(Libbie: "We pick GL from Xero." / William: "Must sync and restrict selection.")* |
| Contacts (Suppliers) | Xero → PO System | Xero | Manual | Suppliers in the PO system map to Xero contacts. Only synced contacts are selectable as suppliers on a PO. *(Libbie: "Vendors = Xero contacts." / William: "Only synced contacts allowed.")* |
| Draft Bill | PO System → Xero | PO System | User-initiated push | A validated PO is pushed to Xero as a draft bill. Reference ID is stored on success. |

- Sync scope is limited to accounts and contacts only. Tracking categories are explicitly excluded. *(William: "Sync accounts and contacts only." / Libbie: "No tracking categories.")*
- Manual synchronization is acceptable for this phase. *(Libbie: "Manual sync acceptable.")*

### 2.4 Access & User Model (Client Confirmed)

- The initial release supports a single superadmin user. *(Libbie: "Only one user for now." / William: "Keep it simple." / Sonal: "Single superadmin login — confirmed.")*
- Multi-user access, user roles, and role-based permissions are deferred to future phases. *(William: "Roles & tracking later phases.")*

### 2.5 Scope Boundaries

**In Scope (This Phase):**

- PO creation with header fields (organization, department, supplier, date, notes, attachments) and line items (description, quantity, unit price, total, GL account)
- Draft save functionality for incomplete POs
- Manual sync of GL accounts from Xero
- Manual sync of contacts/suppliers from Xero
- One-way push of validated PO to Xero as a draft bill
- Storage of Xero reference ID on successful push
- Pre-push validation (required fields, active integration check)
- Single superadmin user login

**Out of Scope (Confirmed for Future Phases):**

- Auto-approval or status updates of bills in Xero
- Tracking categories synchronization
- Multi-user access and role-based permissions
- Automated or real-time synchronization of accounts/contacts
- Reporting or analytics dashboards
- Two-way data sync between PO system and Xero
- Editing or deleting draft bills in Xero from the PO system

### 2.6 Business Rules & Validations (Client Confirmed)

| # | Business Rule | Source |
| :---: | :--- | :--- |
| BR-1 | Draft bills pushed to Xero must never be auto-approved. | Libbie (07:10) |
| BR-2 | Push operation is blocked if any required PO field is missing or incomplete. | William (33:30) |
| BR-3 | Only GL accounts synced from Xero are available for selection on PO line items. | William (18:15) |
| BR-4 | Only contacts synced from Xero are available for selection as PO suppliers. | William (22:00) |
| BR-5 | The Xero integration must be active and validated before a push operation is permitted. | William (13:40) |
| BR-6 | Finance reviews and approves all bills exclusively within Xero. | Libbie (38:15) |
| BR-7 | The system must store the Xero-returned reference ID against the PO after successful push. | Libbie (33:30) |
| BR-8 | Users can save a PO as draft without triggering a push to Xero. | William (26:45) |

---

## 3. Solution Suggested

Satva Solutions proposes building a web-based PO Management application integrated with Xero via the Xero API. The application will provide DSB with a streamlined workflow: create purchase orders internally, validate completeness, and push them to Xero as draft bills in a single action — eliminating the need for duplicate manual data entry.

### 3.1 PO Creation & Draft Save

The system will provide a form-based interface for creating purchase orders. Users will fill in header-level fields (organization, department, supplier, date, notes, file attachments) and add one or more line items (description, quantity, unit price, calculated total, GL account). Supplier and GL account fields will present dropdown selections populated exclusively from the most recent Xero sync. Users may save a PO as a draft at any point, allowing partial completion and later editing before push.

### 3.2 Xero Integration Layer

The integration layer will connect to the Xero API to perform three core operations:

1. **Sync GL Accounts** — Pull the chart of accounts from Xero and store locally. The user triggers this sync manually. Only synced accounts appear in the GL dropdown during PO creation.
2. **Sync Contacts** — Pull contacts from Xero and store locally. The user triggers this sync manually. Only synced contacts appear in the supplier dropdown during PO creation.
3. **Push Draft Bill** — Convert a validated PO into a Xero draft bill via the Xero Bills API. On success, store the Xero-returned reference ID. On failure, display a clear error message indicating the cause (missing fields, integration inactive, Xero API error).

Before any push operation, the system will verify that (a) the Xero integration is active, and (b) all required PO fields are populated. If either check fails, the push is blocked and the user receives a specific validation message.

### 3.3 Single User Model

The initial release will support a single superadmin login. No role-based access control, user management, or multi-tenancy features are included. This approach keeps the first delivery focused and aligned with DSB's current single-user workflow.

### 3.4 Finance Handoff

Once a draft bill is created in Xero, all further actions (review, editing, approval, payment) are performed by DSB's finance team directly within Xero. The PO system does not modify, update, or monitor the bill after push. Xero remains the authoritative system for accounting records.

---

## 4. Development Flow

The project will follow Satva's standard agile delivery methodology, progressing through clearly defined phases from discovery through go-live. The flow confirmed during the discovery call is: Discovery → Development → Demo → Staging → UAT → Production.

- Kickoff meeting to discuss requirements with Satva's development team, including hosting and deployment expectations.
- Xero integration configurations will be completed (API setup, OAuth authentication, initial sync).
- Satva's development will proceed as per the sprint plan.
- Once the first sprint is completed, the Project Manager will plan an internal demo.
- Internal demo with Satva team stakeholders (BA, TA & TL, Dev, QA).
- Staging deployment and release on DSB's staging environment — as per the agreement.
- Staging demo with DSB (Q&A session).
- Accept minor changes and release updated patch on staging.
- Bug fixing from the UAT phase.
- UAT sign-off by DSB's team.
- Production deployment — as per the agreement.
- Support and maintenance — as per the agreement.
- Go Live.

---

## 5. Assumptions/Constraints

The following assumptions and constraints govern the scope, delivery, and operation of the PO Management + Xero Draft Bill Integration system. Items marked **"Client Confirmed"** were explicitly agreed upon during the discovery call. Items marked **"Assumed — To Confirm"** are reasonable inferences that require DSB's written confirmation before development begins.

### 5.1 Assumptions (Client Confirmed)

| # | Assumption | Source |
| :---: | :--- | :--- |
| A-1 | DSB will provide Xero API access credentials and required permissions for integration. | Sonal/William (51:20) |
| A-2 | Finance team finalizes and approves all bills directly within Xero. | Sonal/William (51:20) |
| A-3 | The system operates under a single superadmin user model for the initial phase. | Sonal/William (51:20) |
| A-4 | Manual sync of accounts and contacts is acceptable (no automated real-time sync required). | Libbie (42:20) |
| A-5 | Tracking categories and role-based access are deferred to later phases. | William (45:10) |
| A-6 | Xero remains the source of truth for all accounting data. | William (02:40) |

### 5.2 Assumptions (To Confirm)

| # | Assumption | Clarification Needed |
| :---: | :--- | :--- |
| A-7 | File attachments on POs will be pushed alongside the draft bill to Xero (subject to Xero API attachment limits). | Confirm attachment handling expectations. |
| A-8 | PO numbers will be system-generated using an auto-incrementing sequence. | Confirm whether DSB requires a custom PO numbering format. |
| A-9 | The PO system will be deployed as a cloud-hosted web application. | Confirm hosting and deployment preferences. |
| A-10 | No historical PO data migration is required; the system starts fresh. | Confirm whether existing PO data needs to be imported. |
| A-11 | No reporting or analytics dashboards are required for the initial phase. | Confirm reporting expectations. |

### 5.3 Constraints

| # | Constraint | Rationale |
| :---: | :--- | :--- |
| C-1 | Draft bills must never be auto-approved in Xero. | Client mandate — financial approvals remain in Xero's workflow. |
| C-2 | Push is blocked if required data is missing. | Data integrity — no incomplete bills in Xero. |
| C-3 | Supplier selection is restricted to synced Xero contacts only. | Consistency — prevents orphaned supplier references. |
| C-4 | GL account selection is restricted to synced Xero accounts only. | Consistency — ensures valid chart of accounts mapping. |
| C-5 | Integration must be active before push is allowed. | Operational safety — prevents failed API calls. |
| C-6 | Integration scope is limited to accounts and contacts; tracking categories are excluded. | Client-confirmed scope boundary. |

---

## 6. Next Steps

This BRD is now ready for DSB's review and approval. The following actions are required to move the project forward from discovery into active development.

- DSB (Libbie and William) to review the requirements, scope boundaries, business rules, assumptions, and constraints documented above.
- If required, request changes in scope or requirements at this stage. In such cases, the Satva team will provide a revised ballpark estimate.
- Clarify the items listed under "Assumptions — To Confirm" (A-7 through A-11) and the Open Questions below.
- If all requirements and scope align with DSB's expectations, kindly provide written approval of this BRD.
- After approval, the Pre-sales team will initiate the internal project kick-off meeting with the development team for knowledge transfer.
- The development team will analyze the details shared by the pre-sales team and prepare technical questions for DSB.
- The Project Manager will introduce the Development team to Basecamp and schedule an external project kickoff with DSB.

---

## Open Questions

The following items were not addressed during the discovery call and require clarification before or during sprint planning:

| # | Question | Category | Impact |
| :---: | :--- | :---: | :--- |
| OQ-1 | What level of Xero API access does DSB currently have? Are there plan-level restrictions (e.g., demo vs. production tenant)? | Integration | Determines API setup and rate limit handling. |
| OQ-2 | How should file attachments be handled during push to Xero? Are there size or file-type restrictions DSB wants enforced? | Functional | Affects PO form validation and Xero API attachment logic. |
| OQ-3 | Does DSB require a specific PO numbering format (e.g., prefix, fiscal year, department code), or is auto-incrementing acceptable? | Functional | Impacts PO creation logic and display. |
| OQ-4 | How should the PO date map to Xero bill fields? Is the PO date used as the bill date, due date, or both? | Integration | Determines field mapping during push. |
| OQ-5 | How should the PO notes field map to Xero? Should it populate the bill reference, narrative, or a custom field? | Integration | Determines field mapping during push. |
| OQ-6 | What is the expected notification mechanism when a push fails? In-app message only, or email notification as well? | Non-functional | Impacts error handling and notification design. |
| OQ-7 | Does DSB need to import any existing PO data, or will the system start with a clean slate? | Data Migration | Determines whether a migration module is needed. |
| OQ-8 | What are DSB's hosting preferences — cloud-hosted (SaaS), DSB-managed infrastructure, or Satva-managed hosting? | Deployment | Affects infrastructure planning and cost estimation. |
| OQ-9 | Are there any reporting or dashboard requirements for this phase (e.g., PO status summary, push history log)? | Reporting | Not discussed in discovery; needs confirmation. |
| OQ-10 | Does DSB require an audit trail (e.g., who created/modified a PO, when it was pushed, sync history)? | Non-functional | Impacts data model and logging design. |
