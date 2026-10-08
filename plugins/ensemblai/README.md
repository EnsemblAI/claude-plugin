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
`https://mcp.ensemblai.com/mcp`, and returns the result. The plugin holds no
API key. If you sign in (`/mcp` in Claude Code), Claude Code keeps the sign-in
tokens and sends the access token with every request. Without signing in the
server applies the free trial, counted per IP address, which it stores only as
a hash.

## Free trial and Pro

Without signing in you get the free trial: 10 calls a day on five read tools
(`search_packages`, `get_package_details`, `top_downloads`, `compare_packages`,
`time_series_for_packages`), up to 10 rows per result and 6 months of monthly
data. The other tools are listed, and calling one asks you to sign in. A
signed-in Free or Standard account keeps the same trial, with 10 calls a day of
its own.

Signed in with a Pro account (run `/mcp` in Claude Code and choose the
EnsemblAI server, or `claude mcp login plugin:ensemblai:ensemblai` in a
terminal) the same connection serves all 32 tools with your plan's limits
(5,000 calls a day), weekly data and full history. Seven of those tools
add, change or delete your own watchlist entries, alerts and ensembles; they
are marked destructive, so Claude asks before running them.

For scripts and CI, where nobody can open a browser, connect with a Pro API key
instead: https://www.ensemblai.com/docs/agents

## Links

- Setup for other clients: https://www.ensemblai.com/docs/agents
- Privacy policy: https://www.ensemblai.com/privacy
- Terms: https://www.ensemblai.com/terms
- Support: support@ensemblai.com

MIT licensed.
