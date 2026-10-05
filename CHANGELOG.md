# Changelog

All notable changes to the MrScraper Grok Build plugin are documented here. The
project follows [Semantic Versioning](https://semver.org/).

## 0.1.0 - 2026-10-05

- Added the initial MrScraper Grok Build marketplace, hosted MCP connection,
  and skill pack.
- Connected to `https://mcp.mrscraper.com/mcp` over Streamable HTTP with OAuth
  2.1 browser sign-in; Grok Build registers its own OAuth client and requests
  the `scrape:read`, `scrape:write`, and `account:read` scopes advertised by
  the server, plus `offline_access` for token refresh.
- Included the `mrscraper`, `mrscraper-fetch`, `mrscraper-scrape`, and
  `mrscraper-serp` skills unchanged from the MrScraper Claude plugin 0.1.4.
