# Owned Actor contracts

Identity, default-build OpenAPI schemas and pricing were read from Apify on 2026-10-11 using official Apify CLI 1.8.0. The [dated registry](actors.json) and schema files are snapshots, not a promise that an upstream source is currently healthy. Refresh metadata and schema before each paid plan; verify output fields from that run before mapping. No Actor was deployed or modified by this plugin.

| Source | Owned Actor | Fixed Actor ID | Reviewed build |
| --- | --- | --- | --- |
| Reddit | [Reddit Posts Search Scraper](https://apify.com/khadinakbar/reddit-posts-search-scraper) | `hvkaKLQvHrNYLqmgZ` | `0.1.6`, `sMlUVl1QPxO6O8fab` |
| X | [X Twitter Search Scraper](https://apify.com/khadinakbar/x-twitter-search-scraper) | `6xhgv9qnX0z36Btrv` | `1.1.4`, `pZ39e8acQKkBii6H5` |
| LinkedIn | [LinkedIn Post Search Scraper](https://apify.com/khadinakbar/linkedin-post-search-scraper) | `yMNWUCINasvKFmMED` | `1.0.11`, `aAGw0s76JbqJnDzJ7` |
| Upwork | [Upwork Job Scraper](https://apify.com/khadinakbar/upwork-job-scraper) | `qbfi3XcSM5ySFGkvq` | `1.2.4`, `OaXif0haYgfjqRrsY` |

## Source selection

Choose Reddit for product recommendations and detailed unmet service needs; X for short, explicit purchase or provider requests; LinkedIn for business vendor requests visible in public indexed posts; Upwork for posted contract projects. Start with the two most relevant sources, not every source. LinkedIn indexing can lag; Upwork jobs are opportunities with potentially anonymous clients, not verified company contacts.

Never collect private messages, private group content, audience/member rosters or unrelated profiles. Use each source's intended access and reply paths. Do not ask for source login cookies or vendor API credentials. The Actors' configured provider routes can fail or change, and coverage is best-effort.

## Reddit

Schema: [reddit-posts-search-scraper.json](schemas/reddit-posts-search-scraper.json). Example input: [reddit-input.json](examples/reddit-input.json).

Required `queries` is an array. This plugin limits it to 1–3 focused queries, non-empty strings, and no more than 3 chosen subreddits. The Actor searches every query × subreddit combination; show that fan-out in the plan. `maxPosts` is a total output cap (Actor range 1–200000; plugin default 25). Use `sort: "new"`, `timeRange: "month"` or `"week"`, `includeTopComments: false`, `includeNsfw: false`, and an explicit ISO `postDateLimit` when required. Do not enable optional Reddit client ID/secret inputs. The default proxy configuration can entail Apify residential proxy charges; inspect current costs instead of assuming event-only billing.

Map post `id`, `title`, `body`, `permalink`, `createdAt`, `scrapedAt`, `searchQuery`, and `author` when actually present. `permalink` must be the post discussion URL, not an external article link. Reject deleted/removed body text as evidence unless the remaining title independently substantiates the request. Comment previews are disabled; never attribute a comment's request to the post author.

## X

Schema: [x-twitter-search-scraper.json](schemas/x-twitter-search-scraper.json). Example: [x-input.json](examples/x-input.json).

Required `queries`: 1–50 in the Actor, limited here to 1–3 focused queries. `maxResults`: 1–5000, default here 25 total. Set `searchType: "Latest"`, `maxPagesPerQuery: 2`, `includeRaw: false`, `dedupeResults: true`. Add explicit `since:YYYY-MM-DD`, `until:YYYY-MM-DD` and `lang:en` operators only when they match the user's requested scope. Preserve the submitted dates; operator filters do not prove the returned publication time.

Require `recordType: "tweet"`, an actual `tweetId` or valid status URL, and `text` containing the buying request. Map `tweetUrl`, `createdAt`, `scrapedAt`, `authorUsername` and `searchQuery`. The Actor can return profiles, media, lists or unknown records. Existing same-build readback returned list records despite a successful run; never qualify them as buyers. Do not construct a tweet URL from unrelated numeric IDs or use a list's creation date as the request date.

## LinkedIn

Schema: [linkedin-post-search-scraper.json](schemas/linkedin-post-search-scraper.json). Example: [linkedin-input.json](examples/linkedin-input.json).

`query` is a single string, 2–200 characters. Each additional query requires a separately authorized run and charge reservation. `maxResults`: 1–500, default here 25. Use `datePosted: "last-month"` or `"last-week"`, `maxProviderPages: 2`, `includeComments: false`, `outputMode: "compact"`. These are Google-indexed public post results, with incomplete and sometimes delayed coverage. Do not promise exhaustive LinkedIn access.

Map `postId`, `postUrl`, `description`, `publishedAt`, `scrapedAt`, `authorName`, `authorUrl` and `searchQuery`. A description can be a snippet; mark `evidenceCompleteness: "snippet"` when the full request context is unavailable. Missing or ambiguous `publishedAt` stays unknown. Do not infer buying intent from a headline, engagement count or author title alone. Public comment requests are outside the default post-only scope.

## Upwork

Schema: [upwork-job-scraper.json](schemas/upwork-job-scraper.json). Example: [upwork-input.json](examples/upwork-input.json).

Use a non-empty `searchQueries` array with 1–3 project/service queries. The schema has no top-level required array, but an actual search query or valid start URL is needed. This plugin uses keyword queries rather than arbitrary URLs. `maxResults`: 1–1000, default here 10. Set `maxPagesPerQuery: 1`, `sortBy: "recency"`, `jobType: "all"`, `paymentVerified: false`, `minClientSpend: "any"`, unless the user asks for narrower filters. Setting payment verification can filter useful opportunities; it does not prove legitimacy or intent by itself. Do not enable the optional `cookies` input. The default is an Actor-managed authenticated provider route; access failures require recovery, not cookie extraction.

Map `jobId`, `title`, `description`, `jobUrl`, `postedAt`, `postedAtRaw`, `budget`, `hourlyRange`, `fixedPrice`, `skills`, `clientCountry`, `clientPaymentVerified` and `clientSpend` only when present. `postedAtRaw` is retained as source text; parse a relative date only against a known collection timestamp, retaining uncertainty. Missing client identity remains unknown. A budget is the posting's stated range, not evidence of willingness to pay the user's price. Use the job's on-platform proposal route; do not uncover hidden client contact details.

## Input and drift gates

Validate input against the fresh schema and these tighter workflow limits. Never include unsupported keys, optional credentials, unlimited counts, unrelated scraping modes or undeclared enrichment. All caps are totals for one Actor invocation; provider-page limits do not imply an exact count of external requests. Source searches may return fewer rows.

Refresh the reviewed build if the default build changes. Inspect the new schema and output contract before approving a new plan. Pin the accepted build number in the launch query parameter. Do not silently run `latest` after validating a different build. A schema read proves input compatibility, not output quality or live buyer demand.

Source errors, summary records, metadata-only objects and records without a supported source permalink belong in source-health reporting, not the lead count. Preserve the original pages for audit and report incomplete coverage.
