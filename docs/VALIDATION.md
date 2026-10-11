# Validation evidence

Version: 1.0.0. Date: October 11, 2026.

## Source and packaging

Passed with Claude Code 2.1.289: strict native plugin manifest validation and strict repository marketplace validation. Four skills, zero commands, zero agents, zero hooks and zero MCP servers were inventoried. No runtime or package manifest is included.

Passed: all JSON parses; all four skill frontmatter records; all four example inputs against freshly retrieved Actor input schemas, with unknown-key and optional-credential-input exclusions; 37 local Markdown reference links resolve within the plugin. The source has 32 regular files. File-size, system-file, heuristic credential and private-absolute-path checks passed. These are deterministic source checks, not model-behavior evaluations.

Clean archive round-trip verification is recorded in the release receipt outside the plugin. User data, raw cloud outputs, authentication material and review logs are excluded from the release.

## Host behavior

A bounded offline Claude Code evaluation was attempted with the exact plugin directory, only Read/Skill tools, no MCP servers, no session persistence and no paid Actor or sending capability. It could not execute: the local Claude OAuth session is expired and could not be refreshed. The model evaluation is **not run**; no pass, natural-discovery success or behavioral lift is claimed. User login renewal is required for this check.

The sixteen synthetic fixture cases and the five positive/three negative reviewer invocation cases are prepared. They specify expected behavior and are available for review; fixture files are not executed model results.

## Apify evidence

Authenticated read-only metadata and default-build OpenAPI schema retrieval succeeded for all four owned Actors. Current identities/builds and dated pricing are in the registry. A same-build X dataset shape read found list records in a SUCCEEDED run; the qualification contract excludes them. No new paid Actor run was launched. No live buyer discovery, conversion or source-wide coverage claim is made.

## Distribution

GitHub publication, Anthropic portal validation, submission, security scanning, reviewer state and live listing are recorded separately in the release receipt. None is established by local validation. A directory listing remains subject to Anthropic's review.
