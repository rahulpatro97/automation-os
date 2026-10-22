\# Airtable schema (Proposed)



\## Tables

1\. Contacts: name, email, phone, source, opt-in WhatsApp (bool, date, method), company link

2\. Companies: name, website, size, industry, enrichment notes

3\. Opportunities: contact, offer (Workshop / DIY / DFY Diagnostic / Workflow Intelligence Sprint), stage, score, score reasoning, owner, next step, next step date

4\. Workshop Registrations: contact, cohort, paid (bool), WhatsApp joined, attended day 1, attended day 2, segment

5\. Client Engagements: client, offer, start date, milestones, status, last update sent

6\. Drafts and Approvals: related record, channel, draft text, status (Draft ready / Approved / Rejected / Sent), approver, decision time, rejection note

7\. Feedback: client, milestone, score, comments, date

8\. Audit Log: timestamp, requested\_by (Claude task ID / workflow ID / person), action, input summary, result, error



\## Opportunity stages (allowlist)

New → Claude reviewing → Qualified / Disqualified → Call booked → Call held → Proposal sent → Won / Lost



\## Rules

\- Every automated write produces an Audit Log row.

\- No status changes outside the allowlist; n8n rejects them.

\- Opt-in fields are required before any WhatsApp send.

