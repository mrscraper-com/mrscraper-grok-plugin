# Changelog

All notable changes to the MrScraper Grok Build plugin are documented here. The
project follows [Semantic Versioning](https://semver.org/).

## 0.2.1 - 2026-10-22

- Updated the `mrscraper` skill and the docs for the hosted server's three
  scopes: `scraperAPI` needs `scrape:write`, `webSearch` needs `scrape:read`,
  and `status` needs `account:read`. If `scraperAPI` returns 403
  `insufficient_scope`, sign in again so Grok Build requests `scrape:write`.
  Skills are identical to the MrScraper Claude plugin 0.2.2.

## 0.2.0 - 2026-10-14

- Updated the skills for the hosted MCP server's new tools: `scraperAPI`
  (pages as HTML, Markdown, or screenshots), `webSearch` (Google), and `status`
  (account usage). The `fetch`, `scrape`, `serp`, `rerun`, `results`, and
  `result` tools no longer exist.
- Removed the `mrscraper-scrape` skill. Skills are unchanged from the
  MrScraper Claude plugin 0.2.0.

## 0.1.0 - 2026-10-05

- Added the initial MrScraper Grok Build marketplace, hosted MCP connection,
  and skill pack.
- Connected to `https://mcp.mrscraper.com/mcp` over Streamable HTTP with OAuth
  2.1 browser sign-in; Grok Build registers its own OAuth client and requests
  the `scrape:read`, `scrape:write`, and `account:read` scopes advertised by
  the server, plus `offline_access` for token refresh.
- Included the `mrscraper`, `mrscraper-fetch`, `mrscraper-scrape`, and
  `mrscraper-serp` skills unchanged from the MrScraper Claude plugin 0.1.4.
