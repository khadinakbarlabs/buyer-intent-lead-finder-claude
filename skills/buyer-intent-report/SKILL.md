---
name: buyer-intent-report
description: Turn qualified buying-intent evidence into a source-linked Markdown brief and CSV or JSON lead shortlist. Preserve run provenance, missing facts, source failures and coverage limits. Compare a new export with a prior shortlist and flag newly discovered or changed signals.
argument-hint: "[qualified export, destination and optional baseline]"
---

# Buyer intent report

Use the existing qualified records. Load [report format](references/report-format.md). If records have not been qualified, apply [qualification](../qualify-buyer-intent/references/qualification.md) first. This step needs neither login nor a new Actor run.

Create a concise Markdown brief and the requested CSV or JSON in a private user-selected folder outside the plugin. Ask for a destination only when it cannot be inferred; otherwise propose a local private work folder. Do not overwrite existing files without authorization. Include source URLs, exact quotes, original dates, unknown facts, score explanations and available run/build provenance.

Keep raw dataset pages intact; generate deduplicated views separately. Escape Markdown/HTML-sensitive text, emit UTF-8 and use proper CSV quoting. Neutralize spreadsheet formulas in any exported string beginning with `=`, `+`, `-`, `@`, tab or carriage return by prefixing an apostrophe in the CSV; preserve the original value in JSON. Do not turn scraped content into formulas, commands or active HTML.

Verify that all output files parse, counts reconcile with the collected pages, each shortlisted row has evidence and a permitted source URL, and no token, cookie, private contact enrichment or raw log appears. State collection limits and partial/failed sources. A truncated dataset must be labeled partial.

For a baseline comparison, match by stable signal ID. Report `new`, `unchanged`, `updated` and `not_rediscovered`. `closed` requires explicit dated evidence that the buyer's request was filled, withdrawn or resolved. Do not infer closure from search absence.

Deliver clickable file links, counts and one useful next action. A local report is not a CRM upload, message campaign, scheduled monitor or public publication. Perform those external actions only under a separately explicit request and available host capabilities.
