# Official Apify CLI workflow

The plugin contains instructions and reference data. The separately installed official `apify-cli` executes searches through Apify's API. There is no packaged runtime, local server, setup hook or invented buyer-intent executable. Commands below were inspected in Apify CLI 1.8.0 on 2026-10-11. Recheck installed help rather than assuming flags on another version.

## Availability and authentication

Local imports require no CLI or account. Before an authorized cloud operation:

```sh
apify --version
apify api --help
jq --version
apify info > /dev/null
```

If missing, explain the separate dependencies. The official npm package is `apify-cli`; the verified version can be installed with `npm install -g apify-cli@1.8.0` when the user authorizes setup. Do not use an unpinned package launcher, silently install/upgrade or replace global tools. `jq` is used only for local JSON inspection; an already available equivalent is sufficient. If the installed CLI lacks `api`, `--body -` or `--params`, stop with `CLI_INCOMPATIBLE` and provide an official setup path.

The user completes `apify login` in their own terminal when needed. Existing local login is enough when a narrow authenticated read succeeds. Never run a token-printing command, inspect auth files, or ask for keys/cookies in chat. The official CLI uses the user's login only with Apify; this workflow never extracts its credentials.

## Inspect identity, build, schema and price

Example for the fixed Reddit Actor; repeat only for the selected fixed IDs in [Actor contracts](actor-contracts.md):

```sh
apify api GET /v2/actors/hvkaKLQvHrNYLqmgZ \
  | jq '.data | {id,name,username,taggedBuilds,pricingInfos}'
apify api GET /v2/actors/hvkaKLQvHrNYLqmgZ/builds/default/openapi.json \
  | jq '{buildId:.info["x-build-id"],inputSchema:.components.schemas.inputSchema}'
```

Verify owner `khadinakbar`, exact ID/name and reviewed build. From `pricingInfos`, select the latest effective `startedAt` at or before now, not a scheduled future price. Inspect all charge events, startup charges, minimum charge requirements and platform-usage treatment. Dated prices are in [actors.json](actors.json); they must be refreshed for the plan. A missing flag is unknown, not free usage.

Estimate event fees as planned event count × current event price, plus startup events at the selected memory size (at least one startup event). Explain any additional platform, residential proxy, managed provider or account costs. `maxTotalChargeUsd` caps Actor charge events; do not promise it caps every platform/provider charge or subscription cost. If the user's all-in budget cannot be supported by the provider controls and known cost allowance, stop and explain the missing bound before launching.

Keep a private plan with source/Actor/build, exact input, count/page caps, expected event fees, each run's USD charge cap, total authorized USD budget, allowance for additional costs, and approval scope. Multiple queries on LinkedIn are multiple runs. Reserve each cap before a POST. Do not release an active or unknown reservation based on an estimated charge or an interruption.

## Launch the reviewed plan

Work in a private user-selected directory outside the plugin, with private permissions. Use a distinct file name for every attempt. POSIX example `umask 077` and `set -C` prevent public permissions and accidental overwrites; use equivalent controls on other hosts.

Write the approved input as `input.json`; the [example inputs](actor-contracts.md) are starting points, not approved requests. Validate schema, bounds and required search keys locally. The following shows a single Reddit run; **$0.25 is an example, not spending authorization**. Replace it with the actual authorized per-run amount and use the exact accepted build:

```sh
apify api POST /v2/actors/hvkaKLQvHrNYLqmgZ/runs \
  --body - \
  --params '{"maxTotalChargeUsd":0.25,"timeout":600,"build":"0.1.6"}' \
  < input.json > run-start.json
```

Check the command exit status and validate the saved JSON envelope. Save `.data.id` immediately in the private receipt, along with Actor/build, input path, cap and creation time. Do not interpolate user phrases, scraped content or arbitrary URLs into shell commands. IDs must match `^[A-Za-z0-9]+$` and come from trusted Apify metadata; choose endpoints only from this document. Replace placeholders manually with validated IDs, without evaluating source text.

A successful POST proves a submitted run, not successful collection. A timeout, broken connection, empty output or unparseable response can leave an already created run: set `UNKNOWN_EXTERNAL_OUTCOME`, hold its budget reservation and never automatically launch again.

## Read status and recover

```sh
apify api GET /v2/actor-runs/RUN_ID
apify api GET /v2/actors/ACTOR_ID/runs --params '{"limit":10,"desc":true}'
```

Validate `.data.actId` against the selected Actor. Save `id`, `status`, `buildId`, `startedAt`, `finishedAt`, `defaultDatasetId`, `defaultKeyValueStoreId`, `usageTotalUsd` and available charged event amounts/counts. Do not print broad account or environment details.

Poll at most 20 times at 30-second intervals within a 10-minute observation window. Bound each command to 60 seconds using the host's timeout control; if it cannot bound the command, checkpoint instead of starting an indefinite wait. Polling limits do not cancel or relaunch the remote run. Terminal statuses are `SUCCEEDED`, `FAILED`, `ABORTED` and `TIMED-OUT`. Report still-active runs as running and resume by ID.

For an unknown launch, read at most ten recent runs for the exact Actor, then inspect only plausibly matching candidates' metadata and `INPUT` records privately. Match input/build/start window and avoid selecting an unrelated preexisting run. A non-match does not prove no run was created. Keep `UNKNOWN_EXTERNAL_OUTCOME` until resolved; a fresh run requires renewed explicit scope while accounting for the unresolved reservation.

Read-only 429, transient 5xx and network failures allow at most two retries after the first attempt, honoring Retry-After within the window. Do not retry 401/403, invalid input or conflicting state automatically. A Retry-After beyond the remaining window means checkpoint. Provider access failure needs source recovery, never credential extraction or bypassing an access control.

## Collect this run's evidence

Read the dataset and store IDs from this run's metadata:

```sh
apify api GET /v2/datasets/DATASET_ID
apify api GET /v2/datasets/DATASET_ID/items \
  --params '{"format":"json","limit":100,"offset":0}' > page-000.json
apify api GET /v2/key-value-stores/STORE_ID/records/RUN_SUMMARY
apify api GET /v2/key-value-stores/STORE_ID/records/OUTPUT
```

`INPUT` is a private reconciliation record, never public report data. Use a distinct page filename and a recorded offset. Advance the offset only after a successful command, valid JSON array and successful private write; advance by the actual number of returned rows. Preserve raw pages unchanged. Default collection bounds: 100 rows/page, at most 20 pages or 2000 rows, and 10 minutes per invocation, with 60-second command timeouts. Stop at the first bound and save `PARTIAL`, row count, item-count observation/time and next offset. Resume from that offset.

A short page is not complete while a dataset is growing. Claim complete collection only after terminal status, dataset `itemCount`, collected raw-row count and available run summary reconcile. The summary can describe empty, partial, unsupported or failed provider coverage even when Apify reports `SUCCEEDED`. Missing optional `OUTPUT`/`RUN_SUMMARY` is acceptable only on confirmed not-found; authorization, quota and server failures remain errors. A provider next cursor indicates deeper source coverage may exist even when all current dataset rows are downloaded. Continuing it is a new paid operation needing its own approved plan, not dataset pagination.

Collect useful partial rows from failed runs but label them partial. Summary/error objects, X profiles/lists and malformed evidence are never leads. Publish only selected source-health information, not raw logs, secrets or unrelated records. Re-read charges after completion when available; distinguish observed charges from estimates and report missing or still-settling amounts. Never infer free usage from absent charge fields.

## Checkpoint and outcome

Use [receipt format](../../buyer-intent-report/references/report-format.md) for `PLANNED`, `RUNNING`, `PARTIAL`, `COMPLETED`, `FAILED`, `BLOCKED` or `UNKNOWN_EXTERNAL_OUTCOME`. Save the private receipt after each run launch and successful page. A checkpoint gives run ID, last state, saved pages, next offset/read and held budget. Keep it outside source control.

| Condition | Next action |
| --- | --- |
| Missing CLI/login or incompatible flags | Explain official local setup; user handles login. |
| Missing budget or unknown all-in bound | Present the concrete plan and request the missing limit/allowance. |
| Source blocked, diagnostics or failed coverage | Report source failure and partial evidence; no claim of zero demand. |
| Unknown launch outcome | Reconcile read-only; hold budget; no automatic second POST. |
| Count/page/time bound reached | Save partial result and next offset; resume the same dataset. |
| Missing/unsupported dates or fields | Preserve unknowns and watch/reject affected candidates. |

No schedule is created by installing this plugin. A requested recurring scan needs separately configured execution, per-run/period budget, deduplication, source-health handling and an explicit notification destination. Do not invent monitoring capability.
