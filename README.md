# Buyer Intent Lead Finder

![Buyer Intent Lead Finder](assets/icon.png)

Find people asking for what you sell. Buyer Intent Lead Finder helps Claude turn your offer into focused searches, separate buying requests from noise, and build a shortlist you can act on.

It uses Khadin Akbar's Apify Actors for Reddit, X, LinkedIn and Upwork through the separately installed official Apify CLI. The plugin is skills-only: four workflows with references and example inputs. Its live-search route targets Claude Code, where local command execution is available. You can qualify existing exports, make reports and draft replies without an Apify account or any new search spend.

## Try it

> I help ecommerce businesses migrate to Shopify. Find recent requests for a Shopify migration agency on Reddit and X. Start with a query plan and a cost estimate; wait for my budget before running it.

> Qualify this local export against my Shopify migration offer. Remove seller promotions and old requests, and explain why each remaining candidate fits. Do not spend anything.

> Make a Markdown brief and CSV shortlist from these qualified opportunities. Keep the buying quote, source link, original date and unknown details.

> Draft a helpful reply to these three buying requests. Use my actual services and keep each reply ready for me to review.

## What you get

- Focused searches for purchase requests, provider recommendations, contract projects and explicit replacement evaluations.
- Hot and warm candidate shortlists with short evidence quotes, source URLs, original dates and explainable scores.
- Separate watch lists for ambiguous complaints, incomplete snippets and missing dates; seller posts and diagnostics are excluded.
- Local Markdown, CSV or JSON reports, with duplicates consolidated and source failures, costs and collection limits visible.
- Short relevant reply drafts or project proposals. Messages remain drafts.

Search results are candidate opportunities, not verified future customers. The plugin does not reveal private contact details, identify anonymous clients, guarantee source coverage, send messages or create a background monitor.

## Install in Claude Code

Add this repository's marketplace and install the plugin:

```text
/plugin marketplace add khadinakbarlabs/buyer-intent-lead-finder-claude
/plugin install buyer-intent-lead-finder@khadin-buyer-intent
```

Reload the session after installation. Use the natural requests above, or invoke an individual workflow:

```text
/buyer-intent-lead-finder:find-buyer-intent
/buyer-intent-lead-finder:qualify-buyer-intent
/buyer-intent-lead-finder:buyer-intent-report
/buyer-intent-lead-finder:draft-buyer-outreach
```

GitHub installation is available independently of Anthropic directory review. A public repository and local validation do not establish that Anthropic has approved or listed the plugin.

## Set up live searches

Live searches require an Apify account, available account credit/limits, the official Apify CLI, and local command access. Offline work needs none of those. The verified CLI is `apify-cli` 1.8.0. Node.js/npm and a local JSON reader such as jq are needed for the documented CLI path.

If you choose to install that version, use the official npm package:

```sh
npm install -g apify-cli@1.8.0
apify login
```

Complete login in your own terminal. Never paste API tokens, source cookies or credentials into a conversation or a report. The plugin uses your existing official CLI login; it does not bundle credentials or read login files.

Before a new scan, Claude checks the exact Actor's current schema, build and pricing, shows the queries and count limits, and asks for a total spending limit if none is already authorized. Apify charges are separate from this MIT-licensed skills package. Actor charge caps do not necessarily cap all platform, proxy or provider costs. Source availability and pricing can change.

The detailed [CLI workflow](skills/find-buyer-intent/references/apify-cli.md) covers per-run caps, partial collection, timeouts and read-only continuation. The [Actor contracts](skills/find-buyer-intent/references/actor-contracts.md) include all four fixed identities and freshly checked example inputs.

## Sources

| Source | Owned Apify Actor | Best use |
| --- | --- | --- |
| Reddit | [Reddit Posts Search Scraper](https://apify.com/khadinakbar/reddit-posts-search-scraper) | Recommendation requests and detailed service needs. |
| X | [X Twitter Search Scraper](https://apify.com/khadinakbar/x-twitter-search-scraper) | Short public posts explicitly seeking a product or provider. |
| LinkedIn | [LinkedIn Post Search Scraper](https://apify.com/khadinakbar/linkedin-post-search-scraper) | Public indexed business vendor requests; coverage may lag. |
| Upwork | [Upwork Job Scraper](https://apify.com/khadinakbar/upwork-job-scraper) | Relevant posted contract projects, including anonymous clients. |

Identity, default-build schema and pricing snapshots were verified on October 11, 2026. Current execution is best-effort. The plugin preserves missing facts and failed-source state instead of manufacturing results.

## Privacy, support and review

Keep raw exports, reports and operation receipts in a private work folder outside this repository. The plugin has no telemetry or hosted backend. Live query inputs go to Apify and the source/provider route used by the selected Actor; Claude handles conversation content under your Claude settings. See [Privacy](PRIVACY.md), [Terms](TERMS.md), [Security](SECURITY.md) and [Support](SUPPORT.md).

For reproducible review, see [Reviewer guide](docs/REVIEWER-GUIDE.md), [evaluation cases](evals/cases.json) and [validation evidence](docs/VALIDATION.md). Synthetic examples are labeled and contain no real leads. The license covers plugin materials, not rights to third-party source content.

Built by [Khadin Akbar](https://github.com/khadinakbarlabs). Report issues through [GitHub Issues](https://github.com/khadinakbarlabs/buyer-intent-lead-finder-claude/issues), without credentials or private lead data.
