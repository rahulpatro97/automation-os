\# Architecture and decision log



\## Layers

1\. Events: FlexiFunnels forms and orders, calendar bookings, Zoom attendance, WhatsApp replies, payments, Meta lead forms

2\. n8n (Hostinger VPS, Coolify): captures events, writes state, exposes action workflows to Claude via instance-level MCP, fires Claude tasks

3\. Airtable: shared state, queue, approvals, audit

4\. Claude: Projects (interactive), scheduled tasks (batch), Claude Code cloud tasks with API trigger (per event), Claude Code (builds n8n workflows via the repo)

5\. Humans: approvals in Slack, calls, delivery



\## Connections

\- Claude → n8n: n8n instance-level MCP, added as a claude.ai custom connector (requires public HTTPS; Tailscale-only does not work because Claude connects from Anthropic's cloud). Local Claude Code adds it separately with `claude mcp add`.

\- n8n → Claude: (a) Airtable queue + scheduled task, (b) HTTP POST to a Claude Code cloud task's /fire endpoint, (c) direct Claude API call.

\- Shared state: Airtable status field on every record.



\## Status flow for anything outbound

New → Claude reviewing → Draft ready → Approved → Sent → Logged

Draft ready → Rejected (with note) → Claude reviewing



\## Decision log



\### DEC-001 Separation of duties

Status: Proposed

Decision: n8n owns deterministic actions and all external side effects; Claude owns judgment; Airtable is the single shared state.

Reason: Claude is strong at judgment and inconsistent at exact repetition; n8n is the reverse. Putting rule enforcement in n8n protects against wrong or manipulated Claude output.

Review trigger: a judgment step that n8n rules handle better, or a deterministic step that keeps needing exceptions.



\### DEC-002 Airtable as system of record

Status: Proposed

Decision: Airtable holds leads, opportunities, workshop registrations, clients, approvals and the audit log. Notion holds SOPs and knowledge. ClickUp role to be decided.

Review trigger: data volume or permissions Airtable can't handle.



\### DEC-003 Approval loop is deterministic

Status: Proposed

Decision: Claude drafts; n8n posts to Slack; a human approves; n8n sends and logs.

Reason: Rahul's standing rule requires approval of prospect and client messages; the loop removes copy-paste.



\### DEC-004 Default to batch before event-fired

Status: Proposed

Decision: Start every judgment flow as an Airtable queue processed by a scheduled task. Move to API-fired Claude Code tasks only when a delay costs conversions, and to direct API calls only for sub-minute classification.

Reason: Batch uses the subscription, is easiest to debug, and has no research-preview dependency.



\### DEC-005 Instance-level MCP over MCP Server Trigger node

Status: Proposed

Decision: Expose action workflows through n8n's instance-level MCP, not per-workflow MCP Server Trigger nodes.

Reason: one connection, central authentication, per-workflow enable/disable as a kill switch.



\### DEC-006 Production n8n changes only via the repo

Status: Proposed

Decision: Workflow JSON lives in GitHub; Claude Code proposes changes; a human reviews and imports. No MCP-based editing against production.

