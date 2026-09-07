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

Registry download endpoints return one package at a time over a capped
recent window: no rankings, no growth math, no cross-ecosystem view, no
ownership or dependency context. EnsemblAI pre-computes all of that —
multi-year history, leaderboards, month-over-month growth, category-level
aggregates, ownership attribution, dependency risk, normalized licenses —
for both ecosystems, one call per answer.

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
  (downloads are 30-day rolling and include CI traffic; the latest month
  lags real time; a tiny package's +900% growth is noise), which tools are
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
