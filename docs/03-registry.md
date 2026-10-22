\# Registry: n8n action workflows and Claude tasks



\## n8n action workflows

| ID | Name | Purpose | Type | MCP enabled | Status |

|---|---|---|---|---|---|

| WF-001 | get\_lead | Return one lead record | Read | No | Planned |

| WF-002 | update\_lead\_stage | Move a lead to an allowed stage with reason | Write | No | Planned |

| WF-003 | request\_approval | Post draft to Slack with Approve/Edit | Write (internal) | No | Planned |

| WF-004 | send\_approved\_message | Send an approved draft via allowed channel | Send | Never via MCP | Planned |



Note: WF-004 is triggered only by the Slack approval click, never by Claude.



\## Workflow spec template

\- ID / Name / Owner

\- Purpose (one sentence)

\- Trigger (webhook / schedule / form / Slack action)

\- Input schema (field, type, required, allowed values)

\- Validation rules and rejections

\- Idempotency check

\- Allowlists and rate limits

\- Steps

\- Output returned to caller

\- Audit row written

\- MCP description (what Claude sees)

\- Test cases: valid, invalid, duplicate, missing record

\- Rollback / disable method



\## Claude tasks

| ID | Name | Type | Trigger | Connectors | Status |

|---|---|---|---|---|---|

| CT-001 | Lead triage (batch) | Scheduled | Daily | Airtable, n8n, web search | Planned |

| CT-002 | Weekday brief | Scheduled | Weekdays morning IST | Airtable, Slack | Planned |

| CT-003 | Lead triage (event) | Claude Code cloud task, API trigger | n8n fire | Airtable, n8n | Later, after CT-001 is proven |



\## Claude task spec template

\- ID / Name / Owner

\- Prompt (self-contained; for API-fired tasks, reads the record ID from the fire payload)

\- Trigger and cadence

\- Connectors (minimum)

\- Input (Airtable view or payload)

\- Output (fields written + which action workflow it calls)

\- Failure behaviour (status "Needs human" + reason)

\- Audit row

\- Test plan and pass criteria

