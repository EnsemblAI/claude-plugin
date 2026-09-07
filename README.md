# EnsemblAI for Claude Code

Give Claude real data about the software ecosystem. This plugin connects
Claude Code to [EnsemblAI](https://www.ensemblai.com) — download analytics,
growth trends, dependency structure, and corporate ownership for **PyPI and
npm** — and teaches it when and how to use them.

```
/plugin marketplace add EnsemblAI/claude-plugin
/plugin install ensemblai@ensemblai
```

That's it. No API key needed to start: the plugin connects to the free trial
endpoint (10 calls/day). Add a key for full access.

## What you can ask

- *"Is `requests` still the most-used HTTP client, or has `httpx` caught up?"*
- *"Chart openai vs anthropic SDK downloads over the last two years."*
- *"What are the fastest-growing AI/ML packages right now?"*
- *"Audit my requirements.txt — anything abandoned or risky?"*
- *"Which packages does Google actually publish to npm?"*

Without this, Claude answers those from training data — a snapshot that's
stale the day it ships, and silent about anything released since. With it,
Claude measures.

## Why not just check the registry

Registry download endpoints return one package at a time over a capped
recent window: no rankings, no growth math, no cross-ecosystem view, no
ownership or dependency context. EnsemblAI pre-computes all of that —
multi-year history, leaderboards, month-over-month growth, category-level
aggregates (all of AI/ML in one call), ownership attribution, dependency
risk, normalized licenses — for both ecosystems, one call per answer.

## Full access

The trial is capped (10 calls/day, 10-row results, 6-month monthly charts).
For full access — all 32 tools, weekly granularity, complete history,
5,000 calls/day — subscribe to Pro, mint an API key at
[/settings/api-keys](https://www.ensemblai.com/settings/api-keys), and
connect with it:

```
claude mcp add --transport http ensemblai https://mcp.ensemblai.com/mcp \
  --header "Authorization: Bearer ek_live_YOUR_KEY"
```

Setup for Claude Desktop, Codex, Cursor, and raw REST is at
[ensemblai.com/docs/agents](https://www.ensemblai.com/docs/agents).

## What's in here

- **A skill** that teaches Claude when to reach for package data, how to
  read it correctly (downloads are 30-day rolling and include CI traffic;
  the latest month lags real time), and recipes for common jobs like
  evaluating a dependency or surveying a category.
- **An MCP server connection** to EnsemblAI's hosted endpoint.

## Links

[Website](https://www.ensemblai.com) ·
[Agent docs](https://www.ensemblai.com/docs/agents) ·
[API reference](https://api.ensemblai.com/openapi.json) ·
[Python client](https://pypi.org/project/ensemblai/) ·
[Node client](https://www.npmjs.com/package/ensemblai)

MIT licensed.
