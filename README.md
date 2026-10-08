# EnsemblAI for Claude Code

Give Claude real data about the software ecosystem. This plugin connects
Claude Code to [EnsemblAI](https://www.ensemblai.com) — download analytics,
growth trends, and ownership data for **PyPI and npm** — and teaches it when
and how to use them.

```
/plugin marketplace add EnsemblAI/claude-plugin
/plugin install ensemblai@ensemblai
```

No account needed to start: it connects to `https://mcp.ensemblai.com/mcp`,
which serves a free trial (10 calls/day, no sign-up). To use your own account,
run `/mcp` in Claude Code, choose the EnsemblAI server and sign in: with a Pro
account the **same connection** serves all 32 tools. There is no key to copy.

## What you can ask, out of the box

These work on the free trial, without signing in:

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
leaderboards, month-over-month growth, whole-segment series across 20 domains
and 80 categories, ownership for 2,900+ companies, dependency graphs — for
both ecosystems, one call per answer.

The counts are cleaned, and the cleaning is a parameter rather than a promise:
PyPI downloads count distribution files only (metadata-sidecar fetches that
inflate raw logs by up to ~40% are excluded for all time, so year-over-year is
on one basis), CI traffic is stored separately (`ci_downloads` /
`non_ci_downloads`, sortable by `ci_percentage`), and `country_grouping`
(`ALL` / `US` / `ALL_EX_US`) isolates region-skewed traffic. Full methodology:
<https://www.ensemblai.com/llms.txt>.

## Free trial vs Pro — one URL, sign in to switch

Without signing in the plugin runs as the free trial: five read tools
(`search_packages`, `get_package_details`, `top_downloads`,
`compare_packages`, `time_series_for_packages`) with trial caps. The other 27
tools are listed too; calling one asks you to sign in. Sign in with a Pro
account (`/mcp` in Claude Code) and the same connection serves all 32 tools —
growth movers, package health, dependency graphs and audits, ecosystem maps,
company/organization lookups, plus watchlists, alerts and saved ensembles —
with your plan's real limits.

| | Free trial | Signed in, Pro |
|---|---|---|
| Tools that answer | 5 | all 32 |
| Calls | 10/day | 5,000/day |
| Rows per result | 10 | your plan's limits |
| Chart history | 6 months | full multi-year |
| Granularity | monthly | monthly **and weekly** |

The trial's 10 calls a day are shared by everyone on your network address
until you sign in. A signed-in Free or Standard account keeps the same five
tools and caps, with 10 calls a day of its own.

**Not using the plugin?** Any MCP client can connect to the same URL and sign
in the same way:

```
claude mcp add --transport http ensemblai https://mcp.ensemblai.com/mcp
```

**Scripts and CI**, where nobody can open a browser, send a Pro API key
instead (mint one at
[/settings/api-keys](https://www.ensemblai.com/settings/api-keys)):

```
claude mcp add --transport http ensemblai https://mcp.ensemblai.com/mcp \
  --header "Authorization: Bearer ek_live_YOUR_KEY"
```

Setup for Claude Desktop, Codex, Cursor, and raw REST is at
[ensemblai.com/docs/agents](https://www.ensemblai.com/docs/agents).

## What's in here

- **A skill** (`package-intelligence`) that teaches Claude when to measure
  instead of recalling, how to read the numbers without misreporting them
  (the headline `downloads` figure is the latest complete calendar month
  and includes CI traffic on purpose, so it stays comparable to the
  registry; the
  `ci_downloads` / `non_ci_downloads` split is on the metrics tools that
  need a Pro account; the latest month lags real time; a tiny package's +900%
  growth is noise), which tools are
  actually available on this connection, and recipes for common jobs.
- **An MCP server connection** to EnsemblAI's hosted endpoint. You sign in
  to it with your EnsemblAI account; the plugin holds no key.

## Links

[Website](https://www.ensemblai.com) ·
[Agent docs](https://www.ensemblai.com/docs/agents) ·
[API reference](https://api.ensemblai.com/openapi.json) ·
[Python client](https://pypi.org/project/ensemblai/) ·
[Node client](https://www.npmjs.com/package/ensemblai)

MIT licensed.
