# Buyer Intent Lead Finder — build plan

Version: 1.0.0. Target: Claude Code. Architecture: skills-only, with the separately installed official Apify CLI as the execution boundary. No packaged executable, MCP server, hooks, agent, or background service.

The first useful result is an evidence-linked shortlist of people or project postings explicitly seeking a product, service, vendor recommendation, or replacement that fits the user's offer. A request for a new scan first produces a concrete source/query plan and a cost estimate. Local import, qualification, report generation, and outreach drafting work without login or a paid run.

Use four owned sources after live identity and schema readback: Reddit Posts Search Scraper, X Twitter Search Scraper, LinkedIn Post Search Scraper, and Upwork Job Scraper. Broad social keyword matches, seller promotions, profiles/lists, unsupported dates, historical requests, diagnostics, and unresolved provider failures never count as verified buyers. Preserve evidence and label inference.

Four focused skills: find-buyer-intent, qualify-buyer-intent, buyer-intent-report, and draft-buyer-outreach. Detailed Actor contracts, safe CLI instructions, a scoring rubric, and report/receipt formats travel in skill references. Synthetic fixtures establish expected behavior before writing the workflows.

Acceptance: native Claude manifest validation; all reference links resolve within the package; examples fit the freshly retrieved Actor schemas; synthetic reviewer scenarios cover buyer/seller discrimination, missing dates, diagnostics, duplicates, injection, unknown launches, and spend/outreach gates; clean archive and credential checks; supported host discovery when available; public GitHub source readback; directory validation and submission confirmation. Live paid execution and substantive model evaluations are separate evidence, and remain unclaimed until actually observed.

Release destination: github.com/khadinakbarlabs/buyer-intent-lead-finder-claude, main branch. Submit the repository root to Anthropic's directory for Claude Code. Keep legal acknowledgements and actual portal state visible. Directory acceptance and live availability are external outcomes.
