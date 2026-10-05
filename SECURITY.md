# Security Policy

Please report suspected vulnerabilities privately to
[support@mrscraper.com](mailto:support@mrscraper.com). Include the affected
component, reproduction steps, and potential impact. Do not open a public issue
for an unpatched vulnerability or include credentials, tokens, customer data,
or private target content in a report.

The plugin repository contains no MrScraper credentials, hooks, scripts, or
executables. Authentication is handled by Grok Build's OAuth 2.1 MCP connection,
which stores tokens in `~/.grok/mcp_credentials.json` on the user's machine. If
a credential may have been exposed, revoke it through the relevant account or
client immediately and then contact support.
