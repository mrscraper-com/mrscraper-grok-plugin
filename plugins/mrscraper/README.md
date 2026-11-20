# MrScraper

This Grok Build plugin connects to the hosted MrScraper MCP server and adds
focused skills for page retrieval as HTML, Markdown, or screenshots, brand and
structured JSON extraction, web search, and account usage.

When a public URL is known, scrapeMarkdown or scrapeHtml is the normal first
content-acquisition step. It preserves the page response so Grok can inspect
every available detail, answer follow-up questions, and apply reusable local
extraction logic. Even for roughly 100 same-layout pages, concurrent retrievals
followed by one local batch extractor can be faster and more complete than
repeating backend-LLM extraction for every page. Use scrapeJson when the user
asks MrScraper to extract the data, or when prompt-driven extraction is clearly
simpler than local logic.

The plugin connects to `https://mcp.mrscraper.com/mcp` and authenticates through
OAuth 2.1. A MrScraper account is required. Do not paste OAuth tokens or API
keys into Grok.

See the repository [README](../../README.md) for installation, usage, data
handling, update, and support instructions.
