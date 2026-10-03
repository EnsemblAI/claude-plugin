# EnsemblAI package intelligence

Download, growth, ownership and dependency data for PyPI and npm packages, so
Claude measures adoption instead of recalling it from training data. Ask which
HTTP client is most used, whether a library is growing or fading, who maintains
a package, or what the most-downloaded packages in a category are.

## What the plugin contains

- **A skill** (`package-intelligence`) that tells Claude when to use the data
  and how to read it: what the download figures count, how far the latest month
  lags, and why a tiny package's growth percentage is noise.
- **One remote MCP server connection** to `https://mcp.ensemblai.com/mcp`, run
  by EnsemblAI LLC.

The plugin has no hooks, runs no local commands and installs no packages.

## What it sends and fetches

Each tool call sends the arguments Claude passes to it, such as package names,
a search query, or a dependency list you ask it to audit, to
`https://mcp.ensemblai.com/mcp`, and returns the result. If you set an API key,
it is sent with every request as `Authorization: Bearer <key>`; Claude Code
stores it as a sensitive setting. Without a key the server applies the free
trial, counted per IP address, which it stores only as a hash.

## Free trial and Pro

Without a key you get the free trial: 10 calls a day on five read tools
(`search_packages`, `get_package_details`, `top_downloads`, `compare_packages`,
`time_series_for_packages`), up to 10 rows per result and 6 months of monthly
data.

With a Pro API key (`ek_live_...`, from
https://www.ensemblai.com/settings/api-keys) the same connection serves all 32
tools with your plan's limits (5,000 calls a day), weekly data and full
history. Seven of those tools add, change or delete your own watchlist entries,
alerts and ensembles; they are marked destructive, so Claude asks before
running them.

## Links

- Setup for other clients: https://www.ensemblai.com/docs/agents
- Privacy policy: https://www.ensemblai.com/privacy
- Terms: https://www.ensemblai.com/terms
- Support: support@ensemblai.com

MIT licensed.
