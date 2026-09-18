# EnsemblAI for Claude Code

Give Claude real data about the software ecosystem. This plugin connects
Claude Code to [EnsemblAI](https://www.ensemblai.com) — download analytics,
growth trends, and ownership data for **PyPI and npm** — and teaches it when
and how to use them.

```
/plugin marketplace add EnsemblAI/claude-plugin
/plugin install ensemblai@ensemblai
```

No account needed to start: it connects to the free trial endpoint
(10 calls/day, no sign-up).

## What you can ask, out of the box

These work on the free trial, with no key:

- *"Is `requests` still the most-used HTTP client, or has `httpx` caught up?"*
- *"Compare fastapi, flask and django — downloads, stars, quality."*
- *"Chart numpy vs pandas downloads over the last 6 months."*
- *"What are the most-downloaded packages on npm right now?"*
- *"Find me packages for parsing PDFs, then tell me which is biggest."*

Without this, Claude answers those from training data — a snapshot that was
stale the day it shipped, and silent about anything released since. With it,
Claude measures.

## Why not just check the registry

The free endpoints have hard ceilings, measured against the live services on
2026-09-14: **api.npmjs.org silently truncates any range to 18 months** (HTTP
200, correct shape, the series just starts later), its bulk endpoint caps at
128 packages and rejects `@scoped` names, and pypistats.org serves exactly 180
days. Neither has search or aggregation, so "most downloaded in X" or "how is
all of AI/ML trending" cannot be asked at all. EnsemblAI stores the full
history (npm from 2015, PyPI from 2018) and pre-computes the rest —
leaderboards, month-over-month growth, whole-segment series across 72 domains
and 519 categories, ownership for 2,900+ companies, dependency graphs — for
both ecosystems, one call per answer.

The counts are cleaned, and the cleaning is a parameter rather than a promise:
PyPI downloads count distribution files only (metadata-sidecar fetches that
inflate raw logs by up to ~40% are excluded for all time, so year-over-year is
on one basis), CI traffic is stored separately (`ci_downloads` /
`non_ci_downloads`, sortable by `ci_percentage`), and `country_grouping`
(`ALL` / `US` / `ALL_EX_US`) isolates region-skewed traffic. Full methodology:
<https://www.ensemblai.com/llms.txt>.

## Free trial vs Pro

The plugin ships pointing at the trial endpoint, which exposes five read
tools: `search_packages`, `get_package_details`, `top_downloads`,
`compare_packages`, `time_series_for_packages`.

| | Free trial | With a Pro key |
|---|---|---|
| Calls | 10/day | 5,000/day |
| Rows per result | 10 | your plan's limits |
| Chart history | 6 months | full multi-year |
| Granularity | monthly | monthly **and weekly** |

**Add your key in the plugin config** (`api_key` — optional, set during
install) to lift those caps on the five tools above.

**Connect the full endpoint** for all 32 tools — growth movers, package
health, dependency graphs and audits, ecosystem maps, company/organization
lookups, plus watchlists, alerts and saved ensembles:

```
claude mcp add --transport http ensemblai https://mcp.ensemblai.com/mcp \
  --header "Authorization: Bearer ek_live_YOUR_KEY"
```

Keys come with the Pro plan — subscribe and mint one at
[/settings/api-keys](https://www.ensemblai.com/settings/api-keys). Setup for
Claude Desktop, Codex, Cursor, and raw REST is at
[ensemblai.com/docs/agents](https://www.ensemblai.com/docs/agents).

## What's in here

- **A skill** (`package-intelligence`) that teaches Claude when to measure
  instead of recalling, how to read the numbers without misreporting them
  (the headline `downloads` figure is 30-day rolling and includes CI
  traffic on purpose, so it matches what the registry publishes; the
  `ci_downloads` / `non_ci_downloads` split is on the full endpoint's
  metrics tools; the latest month lags real time; a tiny package's +900%
  growth is noise), which tools are
  actually available on this connection, and recipes for common jobs.
- **An MCP server connection** to EnsemblAI's hosted endpoint, with an
  optional API key.

## Links

[Website](https://www.ensemblai.com) ·
[Agent docs](https://www.ensemblai.com/docs/agents) ·
[API reference](https://api.ensemblai.com/openapi.json) ·
[Python client](https://pypi.org/project/ensemblai/) ·
[Node client](https://www.npmjs.com/package/ensemblai)

MIT licensed.
