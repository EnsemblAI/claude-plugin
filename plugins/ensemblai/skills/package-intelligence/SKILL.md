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

## How to pick a tool

- Find packages by topic/keyword → `search_packages`, then
  `get_package_details` / `get_package_metrics` / `get_package_health`
- Most popular → `top_downloads` · Trending/accelerating → `growth_movers`
- Trends and charts → `time_series_for_packages`
- Head-to-head → `compare_packages` (2–10 names)
- Who publishes what → `list_companies` / `list_company_packages`
  (corporate owners) or `list_organizations` / `get_organization_packages`
  (GitHub orgs)
- Ecosystem structure → `ecosystem_map`, `ecosystem_clusters`
- Dependencies → `get_package_dependencies`, `package_depgraph`,
  `audit_dependencies`
- **Before filtering by a domain, category, license, language, owner or
  country, call `reference_values` first** to get the real valid values —
  guessing a filter value returns empty results, not an error.

## Recipes

**Evaluate a dependency before adopting it**
`get_package_details` → `get_package_health` (deprecated? abandoned?
single-maintainer?) → `time_series_for_packages` (is adoption growing or
declining?) → `get_package_dependencies` (what does it drag in?). Report
adoption trend, maintenance signal, and ownership together — a popular but
abandoned package is a different risk from an unpopular but well-maintained
one.

**Compare candidate libraries**
`compare_packages` for the snapshot, then `time_series_for_packages` for
trajectory. The trend usually matters more than today's absolute numbers:
a smaller package growing fast is often the better bet.

**Survey a category**
`reference_values` (get valid domain/category names) → `top_downloads`
filtered to that domain → `growth_movers` for the same domain. That gives
both the incumbents and the challengers.

**Audit a project's dependencies**
Read the local `requirements.txt` / `package.json`, then
`audit_dependencies` with the package list for health flags and known
vulnerabilities.

## Access and limits

- Some capabilities are plan-gated. A call that returns an upgrade/tier
  message is **gated, not broken** — explain what's needed and do not retry
  the same call.
- On the free trial endpoint you have a small daily call budget, results are
  capped at 10 rows, and charts cover 6 months (monthly). When the budget is
  exhausted the tools return a readable message — relay it to the user
  rather than treating it as an error.
- Write tools (watchlist, alerts, ensembles) modify the user's own account.
  Confirm intent before calling them.

## Answering well

- Lead with the number and the direction, then the caveat — not the reverse.
- Name the anchor month when the recency matters.
- Distinguish "we have no data" from "the value is zero"; they mean very
  different things.
- When the data contradicts a common belief (or your own prior), say so
  explicitly — that contrast is usually the most useful part of the answer.
