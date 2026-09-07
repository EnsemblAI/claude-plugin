---
name: package-intelligence
description: Measure real-world adoption of PyPI and npm packages — downloads, month-over-month growth, competitive comparison, corporate ownership, dependency risk, and category trends. Use whenever a question involves how popular, how fast-growing, how healthy, or how widely depended-upon a software package is, instead of answering from (stale) training data.
version: 0.1.0
keywords: [pypi, npm, packages, downloads, adoption, dependencies, ownership, trends]
---

# EnsemblAI package intelligence

Use the EnsemblAI tools for any question about **real-world adoption of
software packages** on PyPI (Python) and npm (JavaScript/TypeScript):
downloads, growth, competitive share, corporate ownership, dependency
structure, and package health.

## When to use this

Reach for these tools — don't answer from memory — whenever the question
involves:

- **Popularity or adoption**: "is X still used?", "what's the most popular
  HTTP client?", "how big is X really?"
- **Trends over time**: "is X growing or dying?", "chart X vs Y", "when did
  X overtake Y?"
- **Competitive comparison**: "should I use X or Y?", "who's winning,
  X or Y?"
- **Category/market questions**: "what are the fastest-growing AI/ML
  packages?", "how big is the vector-database space?"
- **Ownership**: "who maintains X?", "what does Google publish?", "is this
  corporate-backed or a solo project?"
- **Dependency risk**: "what depends on X?", "is X safe to adopt?",
  "audit my requirements file"

Your training data is a *snapshot* and its popularity impressions are often
stale or wrong — a package that was dominant at training time may be in
decline now, and packages released since do not exist in your weights.
These tools return current, measured data. Prefer them over recall.

Some of those questions need tools that are only on the full endpoint —
see "Which tools you actually have" below before planning an answer.

## Why not just query the registries

Registry download endpoints answer one package at a time over a capped
recent window, with no rankings, no growth math, no cross-ecosystem view,
and no ownership, license, or dependency context. Public download datasets
with real history require running (and paying for) warehouse queries over
terabyte-scale tables. EnsemblAI pre-computes all of it — leaderboards,
month-over-month growth, category-level aggregates, ownership attribution,
dependency structure — and answers in milliseconds, one call per question.

## Reading the data correctly

These are the misreadings that produce confidently wrong answers:

- **`downloads` is a rolling 30-DAY count**, not all-time or cumulative.
- **Downloads include CI, mirrors, and automation.** It is a relative
  popularity signal, *not* a count of human users. Never present it as
  "X people use this."
- **The latest month lags real time** (~1 month for PyPI, ~2 for npm), so
  "latest" is not the current calendar month. Say which month the data is
  anchored to when it matters.
- **`mom_pct` is month-over-month percent change** in downloads.
- **Percentages are share of that ecosystem's total** for the period.
- **A tiny package's growth % is noise** — 3 downloads to 30 is +900%.
  Sanity-check absolute volume before reporting a growth headline.
- **`github_stars` / `github_quality_score`** come from a linked GitHub
  repo; not every package has one, and 0 usually means "not computed",
  not "terrible".
- **Cross-ecosystem comparisons are directional, not exact.** PyPI and npm
  count downloads differently; compare trends and shares, not raw totals.

## Which tools you actually have

**This plugin connects to the EnsemblAI endpoint that exposes five tools.**
Check what's available before planning a multi-step answer — the rest of the
catalogue lives on the full endpoint and is NOT callable here.

Available on this connection:

| Tool | Use it for |
|---|---|
| `search_packages` | find packages by topic/keyword/name |
| `get_package_details` | one package's full profile (downloads, growth, license, domain, owner, GitHub signals) |
| `top_downloads` | the leaderboard — most-downloaded packages |
| `compare_packages` | 2–10 packages side by side |
| `time_series_for_packages` | download history for charting/trends |

On the **full endpoint** (`https://mcp.ensemblai.com/mcp`, Pro key) there are
32 tools, adding: `growth_movers` (fastest risers), `get_package_health`,
`get_package_metrics`, `get_package_dependencies`, `package_depgraph`,
`package_cohort`, `audit_dependencies`, `ecosystem_map`,
`ecosystem_clusters`, `reference_values`, company/organization lookups, and
watchlist/alert/ensemble management. If a question needs one of those, say
which tool would answer it and that it needs the full endpoint — don't
attempt the call and don't improvise a substitute silently.

Note `get_package_details` is the workhorse here: it already returns
downloads, growth, license, domain, category, corporate owner and GitHub
stats for one package, so most single-package questions need exactly one
call.

## Recipes (using the five tools available here)

**Is this package healthy / should I adopt it?**
`get_package_details` for the profile, then `time_series_for_packages` for
trajectory. Report adoption level *and* direction together — a package with
big absolute numbers but a steady decline is a different decision from a
smaller one that's climbing. (Deprecation flags and vulnerability scanning
need `get_package_health` / `audit_dependencies` on the full endpoint; say so
rather than guessing.)

**Which of these libraries should I use?**
`compare_packages` for the snapshot, then `time_series_for_packages` on the
same names for the trend. State both, and be explicit that download share is
a popularity signal, not a quality verdict.

**What's popular in this space?**
`search_packages` on the topic to find candidates, then `top_downloads` for
scale, then `compare_packages` on the shortlist. (Domain-filtered
leaderboards and `growth_movers` need the full endpoint.)

**Did X overtake Y, and when?**
`time_series_for_packages` with both names, then read the crossover from the
series rather than asserting it from memory.

## What this connection gives you (and what a key adds)

This plugin talks to EnsemblAI's endpoint in one of two modes, decided by
whether the user has set an API key in the plugin config.

| | Free trial (no key) | Pro (API key set) |
|---|---|---|
| Daily calls | 10 | 5,000 |
| Rows per result | 10 | your plan's limits |
| Chart history | 6 months | full multi-year |
| Granularity | monthly only | monthly **and weekly** |
| Tools here | these 5 | these 5, uncapped |
| Full 32-tool surface | — | at `https://mcp.ensemblai.com/mcp` |

Practical consequences:

- **Weekly granularity requires a key.** If the user asks for weekly data
  without one, deliver the monthly series and say plainly that weekly needs
  Pro — don't silently substitute monthly and let them think it's weekly.
- **6-month history without a key.** For "over the last 2 years" style
  questions on the trial, chart what you can and name the limitation.
- **When the trial budget runs out**, the tools return a readable message.
  Relay it — that's not an error, and retrying won't help.
- **Be specific about what upgrading buys** (weekly data, full history,
  larger results, dependency graphs, ecosystem maps, watchlists and alerts),
  not just "upgrade for more." Setup instructions:
  https://www.ensemblai.com/docs/agents
- A key is added in the plugin's config (`api_key`), or by connecting the
  full endpoint directly with `Authorization: Bearer ek_live_...`.
- Tier-gated calls return an upgrade message rather than data. That means
  **gated, not broken** — explain it and do not retry the same call.

## Answering well

- Lead with the number and the direction, then the caveat — not the reverse.
- Name the anchor month when the recency matters.
- Distinguish "we have no data" from "the value is zero"; they mean very
  different things.
- When the data contradicts a common belief (or your own prior), say so
  explicitly — that contrast is usually the most useful part of the answer.
