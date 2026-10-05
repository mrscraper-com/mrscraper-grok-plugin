# MrScraper for Grok Build

MrScraper connects [Grok Build](https://x.ai/build) to a hosted MCP server for
fetching public web pages, managed structured extraction, Google discovery,
saved scraper reruns, stored results, and account usage. The plugin also
includes four focused skills that help Grok choose the right workflow and
preserve raw page content.

## What this plugin adds

- The hosted Streamable HTTP endpoint at `https://mcp.mrscraper.com/mcp`.
- OAuth 2.1 authentication through Grok Build's MCP client; no credential is
  stored in this repository.
- A fetch-first workflow that keeps raw page responses available for analysis,
  verification, and follow-up transformations.
- Managed extraction, site mapping, Google SERP discovery, saved reruns, and
  stored-result tools when those capabilities are useful.

## Requirements

- A current version of [Grok Build](https://docs.x.ai/build/overview).
- A [MrScraper](https://app.mrscraper.com) account.
- Permission to access and process the target content.

## Install

From the xAI Official marketplace:

```bash
grok plugin marketplace add xai-org/plugin-marketplace
grok plugin install mrscraper --trust
```

Inside a Grok Build session, you can instead type `/marketplace`, search for
`mrscraper`, and press `i`.

If `mrscraper` is not listed in the xAI Official marketplace yet, install it
from this repository's own marketplace, which xAI does not review:

```bash
grok plugin marketplace add mrscraper-com/mrscraper-grok-plugin
grok plugin install mrscraper@mrscraper-grok-plugin --trust
```

`--trust` confirms the installation. Grok Build does not load the skills or
start the MCP server of a plugin you have not trusted.

## Sign in

Start `grok`, open `/mcps`, select `mrscraper`, and press `i`. Grok Build opens
the MrScraper consent page in your browser; sign in and approve access. Grok
Build registers its own OAuth client and requests the `scrape:read`,
`scrape:write`, and `account:read` scopes advertised by the server, plus
`offline_access` so it can refresh the token. Never paste OAuth tokens or API
keys into chat.

Until you sign in, `grok mcp doctor mrscraper` reports that the handshake
failed with `Auth required`. Run it again after sign-in to confirm the
connection.

## Try it

```text
Fetch https://www.scrapethissite.com/pages/simple/ and summarize the page.
```

The connection exposes these MCP tools, shown in Grok Build as
`mrscraper__<tool>`:

| Tool | Purpose |
| --- | --- |
| `fetch` | Retrieve and preserve a known page's raw response. |
| `scrape` | Run managed structured extraction or bounded site mapping. |
| `serp` | Discover public pages through Google. |
| `rerun` | Reuse a saved AI or manual scraper configuration. |
| `results` | Browse and filter stored result records. |
| `result` | Retrieve one stored result or poll an asynchronous run. |
| `status` | Inspect subscription usage and request outcomes. |

## Fetch-first routing

When a public URL is already known, the skills direct Grok to fetch it first
and treat the raw response as the source of truth. Grok can read, summarize,
compare, or derive structured output locally without losing details to an
early extraction prompt.

This preference can still be faster for large same-layout sets. For roughly
100 known pages, Grok can safely fetch pages concurrently, retain every raw
response, and apply one reusable local extractor instead of requesting 100
separate backend-LLM extractions. Use `scrape` when managed extraction is
explicitly requested or has a clear benefit after the page structure and
desired schema are understood.

## Data and permissions

The plugin sends MCP tool inputs—including target URLs and extraction
instructions—to MrScraper's hosted service. Page responses and tool results are
then available to Grok for the requested task. Managed general and listing
extraction sends page content and the extraction prompt to a backend language
model and saves a reusable scraper configuration; reruns create new stored
results. Use it only with public or otherwise authorized content, and follow
the target site's requirements.

Network endpoints and credentials:

- `https://mcp.mrscraper.com/mcp`: the hosted MCP server, reached over HTTPS.
  Tool inputs are sent here.
- `https://api.app.mrscraper.com`: the OAuth 2.1 authorization server
  (authorization, token, and dynamic client registration endpoints),
  advertised in the server's protected resource metadata at
  `https://mcp.mrscraper.com/.well-known/oauth-protected-resource/mcp`.
- `https://app.mrscraper.com`: the MrScraper sign-in and consent pages opened
  in your browser.
- Credentials: a MrScraper account and the OAuth grant Grok Build stores in
  `~/.grok/mcp_credentials.json`. The plugin reads no environment variables or
  files and ships no hooks, scripts, or executables.

Review MrScraper's [MCP documentation](https://docs.mrscraper.com/docs/getting-started/mcp-server),
[Privacy Policy](https://mrscraper.com/privacy-policy), and
[Acceptable Use Policy](https://mrscraper.com/acceptable-use-policy) before use.

## Update or remove

```bash
grok plugin marketplace update
grok plugin update mrscraper
grok plugin uninstall mrscraper
```

If you installed the CLI-oriented MrScraper skills with
`mrscraper init --agent grok`, Grok Build keeps both skill sets and shows
qualified names such as `/user:mrscraper-fetch` and
`/mrscraper:mrscraper-fetch`. Keep only the set that matches how you connect:
this plugin for MCP, or the CLI skills for the `mrscraper` command.

## Support and security

- Product help: [MrScraper MCP documentation](https://docs.mrscraper.com/docs/getting-started/mcp-server)
- Bugs and feature requests: [GitHub Issues](https://github.com/mrscraper-com/mrscraper-grok-plugin/issues)
- Account help: [support@mrscraper.com](mailto:support@mrscraper.com)
- Security reports: see [SECURITY.md](SECURITY.md)

## Development

The repository is both an installable Grok Build marketplace and the source of
the `mrscraper` plugin:

```text
.grok-plugin/marketplace.json
plugins/mrscraper/
├── .grok-plugin/plugin.json
├── .mcp.json
└── skills/
```

Validate the plugin and its marketplace before a release:

```bash
grok plugin validate plugins/mrscraper
grok plugin marketplace add .
grok plugin install mrscraper@mrscraper-grok-plugin --trust
grok inspect
```

The plugin uses semantic versions from its plugin manifest. Bump that version
for every release and record user-visible changes in [CHANGELOG.md](CHANGELOG.md).
Use [PUBLISHING.md](PUBLISHING.md) for the release and xAI marketplace
checklist.

## License

[MIT](LICENSE)
