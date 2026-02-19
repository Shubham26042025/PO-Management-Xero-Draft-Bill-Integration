# **DSB \- Discovery Call Transcript (PO Management \+ Xero Draft Bill Integration)**

**Project:** PO Management \+ Xero Draft Bill Integration

**Company:** DSB

Client Attendees: Libbie, William

**Satva Solutions:** Sonal (Lead BA)

**Meeting Type:** Requirement Discovery

**Mode:** Virtual Call

**Duration:** \~58 Minutes

00:00 Introductions

Sonal: Hi Libbie, hi William — good to connect. Today I want to understand your purchasing workflow and accounting handling.

Libbie: We mainly want to remove duplicate work between our purchasing and Xero.

William: Yes — we re-enter data into Xero manually.

02:40 Current Process

Libbie: We prepare a purchase order → accounting creates bill manually in Xero.

Sonal: So Xero remains the source of truth?

William: Correct.

07:10 Goal

Sonal: Create PO internally and push to Xero for finance review?

William: Yes — as draft bill only.

Libbie: Never auto approve.

10:05 Access

Libbie: Only one user for now.

William: Keep it simple.

Sonal: Single superadmin login — confirmed.

13:40 Integration Scope

William: Sync accounts and contacts only.

Libbie: No tracking categories.

Sonal: Integration must be active before push.

William: Yes.

18:15 GL Accounts

Libbie: We pick GL from Xero.

William: Must sync and restrict selection.

22:00 Suppliers

Libbie: Vendors \= Xero contacts.

William: Only synced contacts allowed.

26:45 PO Fields

Libbie: Org, dept, supplier, date, notes, attachments.

Line items: description, qty, price, total, GL.

William: Draft save allowed.

33:30 Push Behavior

William: Push → create draft bill in Xero.

Libbie: Save reference ID.

William: Block if data missing.

38:15 Finance

Libbie: Finance finalizes inside Xero only.

42:20 Sync

Libbie: Manual sync acceptable.

45:10 Future Scope

William: Roles & tracking later phases.

48:00 Deployment

Sonal: Discovery → Dev → Demo → Staging → UAT → Production.

51:20 Assumptions

Sonal: Provide Xero access, finance finalizes in Xero, single user model.

William: Confirmed.

55:00 Closing

Sonal: I’ll prepare the requirement document.

Libbie: Great.

William: Send for approval.