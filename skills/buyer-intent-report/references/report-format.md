# Report, export and receipt format

Save artifacts in a private user-selected directory outside the installed plugin and outside public repositories. Preserve existing files; use distinct dated names and private permissions. Do not package user results, credentials, raw logs or account details in releases.

## Markdown brief

Begin with the offer, analysis time, requested source/date scope and whether the result is complete, partial, or an offline import. Then show the shortlist:

| Priority | Requested outcome | Buying evidence | Source/date | Fit and uncertainty | Next action |
| --- | --- | --- | --- | --- | --- |
| Hot/warm and score | Concrete requested service/product | Short exact quote, safely escaped | Source hyperlink + original publication date | Supported fit, budget/timing if explicit; unknowns | Review source, draft a relevant platform reply, or inspect a missing field |

Add a compact watch list and rejection summary. Report raw rows collected, eligible source records, unique evaluated signals, hot/warm/watch counts, duplicate count, rejected count and diagnostics count. Make the accounting mutually exclusive: raw rows = diagnostics + unsupported records + duplicates + unique evaluated signals; unique evaluated signals = hot + warm + watch + rejected. If a supplied export cannot support these counts, state which count is unavailable.

Source coverage includes Actor/build/run when available, actual queries and input counts, run/summary outcome, dataset item-count observation, downloaded count, next offset, page/time bounds, provider pagination if reported, original-date limitations and failed sources. Complete dataset download does not mean exhaustive source search. Distinguish observed Actor fees/platform usage from estimates; report unavailable or settling charges. Avoid broad account metadata.

## JSON shortlist

Use this field contract; null means unknown. The provenance is private and can be omitted from a shared version at the user's request, with that omission labeled.

```json
{
  "schemaVersion": "1.0.0",
  "analysisAt": "2026-10-11T10:00:00Z",
  "offer": "User-supplied offer",
  "scope": {"sources": ["reddit"], "maxAgeDays": 30},
  "collectionState": "offline_import",
  "coverage": [],
  "signals": [
    {
      "signalId": "reddit:synthetic-example",
      "source": "reddit",
      "sourceUrl": "https://www.reddit.com/r/example/comments/synthetic-example/",
      "title": "Synthetic example, not a real lead",
      "intentClass": "buyer_request",
      "status": "hot",
      "score": 95,
      "scoreBreakdown": {"intent": 40, "fit": 25, "freshness": 15, "action": 10, "timingBudget": 5},
      "scoreReasons": ["Exact request for the offered service", "Timing is stated; budget unknown"],
      "evidenceQuote": "Looking for a Shopify migration agency this month.",
      "evidenceCompleteness": "full_text",
      "publishedAt": "2026-10-10T12:00:00Z",
      "publishedAtRaw": null,
      "collectedAt": "2026-10-11T09:00:00Z",
      "fitReason": "Requested migration matches the user's supplied offer.",
      "knownBudget": null,
      "knownTiming": "this month",
      "unknowns": ["Company", "Region", "Budget"],
      "contactState": "platform_reply_only",
      "provenance": []
    }
  ]
}
```

For cloud evidence, each provenance entry includes `actorId`, `buildId`, `runId`, `datasetId`, `pageFile`, `rowIndex` and `inputQuery`, obtained from the corresponding saved run/page. For an import without metadata, record the local input file and row index with cloud fields null; never manufacture a run receipt. A row's embedded `runId` does not override the run metadata of the collected dataset.

## CSV

Columns: `signal_id,source,status,score,intent_class,title,evidence_quote,source_url,published_at,collected_at,fit_reason,known_budget,known_timing,unknowns,contact_state,actor_id,build_id,run_id`.

Use UTF-8, quote every field as needed, double embedded quote characters and preserve multiline fields correctly. Before exporting any string with a formula-trigger prefix (`=`, `+`, `-`, `@`, tab, carriage return), prefix it with an apostrophe for spreadsheet safety. Apply this to every string column, not just the evidence quote. Keep original values in JSON and raw pages. Do not emit active HTML or executable content from imported text.

## Private operation receipt

One receipt per scan plan; append/update checkpoints atomically in a private working folder. Required facts:

- `schemaVersion`, `operationId`, `createdAt`, `updatedAt`, `state`, `nextAction`.
- `approvalScope`: selected sources, exact input files, count/page bounds, authorized total USD, each run's cap, additional-cost allowance and the user's authorization reference. Never copy a full chat transcript.
- `runs`: Actor/build, input file, run ID if known, start window, last status, dataset/store IDs if returned, completed page files and next offset, observed item count/time, summary outcome and errors, actual charge observations/time.
- `budget`: reserved USD for active/unknown runs, settled observed charges, additional-cost allowance and remaining authorized amount. Do not free unknown reservations to start another scan.
- `coverageState`: complete dataset download, partial dataset download, failed source, offline import, or unknown external outcome. Preserve provider next-cursor state privately when returned.

Allowed operation states: `PLANNED`, `BLOCKED`, `RUNNING`, `PARTIAL`, `COMPLETED`, `FAILED`, `UNKNOWN_EXTERNAL_OUTCOME`. `COMPLETED` means the requested bounded collection and artifact writes were verified, not that every source was exhaustively searched or a sale occurred. A multi-source plan with a failed source is partial, even if its other runs succeeded.

## Baseline comparison

Compare stable signal IDs and retain original evidence. Use `new`, `unchanged`, `updated`, `not_rediscovered`. Use `closed` only with explicit source evidence and a date. A later partial/failed scan cannot establish that a missing buyer stopped looking. A refresh is a new billable operation unless it uses supplied exports; recurring execution is not part of installation.
