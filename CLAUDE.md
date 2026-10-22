\# automation-os



This repo holds the n8n deterministic layer and the Claude task prompts for Rahul's business. The design source of truth is the claude.ai Project "Automation OS: n8n + Claude"; mirror its decisions here, don't diverge.



Rules:

\- One workflow per file, named WF-###-name.json, with a matching spec in specs/workflows/.

\- Every write workflow: input validation, idempotency check, allowlists, audit row.

\- No workflow accepts free-form recipients or message bodies from Claude.

\- Never commit secrets. Reference n8n credentials by name only.

\- Changes go through a branch and a pull request. Do not edit the production n8n instance directly.

\- Before using any n8n node or Claude feature, check the official docs and note the doc URL in the spec.

\- After any change, update CHANGELOG.md and list the manual test cases to run.

