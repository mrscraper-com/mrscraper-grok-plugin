# Publishing MrScraper for Grok Build

Use this checklist for a public MrScraper Grok Build plugin release and for the
xAI Official marketplace listing.

## Release gate

- [ ] Merge only reviewed changes into `main`.
- [ ] Keep the release version in
      `plugins/mrscraper/.grok-plugin/plugin.json`; do not duplicate it in
      the marketplace entry.
- [ ] Record user-visible changes in `CHANGELOG.md`.
- [ ] Confirm that the repository contains no credentials or private target
      data.
- [ ] Run the validation from a clean checkout:

  ```bash
  grok plugin validate plugins/mrscraper
  grok plugin marketplace add .
  grok plugin install mrscraper@mrscraper-grok-plugin --trust
  grok inspect
  ```

- [ ] Confirm the GitHub validation workflow passes.
- [ ] Install the plugin from the public repository in a clean Grok Build
      profile, complete OAuth from `/mcps`, and confirm all seven MCP tools
      load.
- [ ] Run the smoke-test prompt below.
- [ ] Create the matching Git tag (`grok plugin tag plugins/mrscraper --push`)
      and a GitHub release.

## Submission details

| Field | Value |
| --- | --- |
| Catalog name | `mrscraper` |
| Catalog description | MrScraper web scraping and structured data extraction (MCP Server + Skills). Fetch raw page HTML through MrScraper's Web Unblocker, run managed AI extraction or site mapping, discover pages through Google search, rerun saved scrapers, and inspect stored results and account usage. Sign in with your MrScraper account through OAuth on first connection; no API key to paste. |
| Category | `development` |
| Repository | `https://github.com/mrscraper-com/mrscraper-grok-plugin` |
| Plugin path | `plugins/mrscraper` |
| Homepage | `https://docs.mrscraper.com/docs/getting-started/mcp-server` |
| Keywords | `mrscraper`, `mr scraper`, `mrscraper mcp` |
| Domains | `mrscraper.com`, `app.mrscraper.com`, `docs.mrscraper.com` |
| Support | `support@mrscraper.com` |
| Privacy policy | `https://mrscraper.com/privacy-policy` |
| Acceptable use policy | `https://mrscraper.com/acceptable-use-policy` |
| MCP endpoint | `https://mcp.mrscraper.com/mcp` |
| Authentication | OAuth 2.1 with dynamic client registration |
| License | MIT |

The hosted service receives target URLs and tool inputs, including extraction
instructions. Results are returned to Grok for the user's requested task. The
OAuth connection requests `scrape:read`, `scrape:write`, `account:read`, and
`offline_access`. The plugin contains no static credentials, hooks, or scripts.

Smoke-test prompt:

```text
Fetch https://www.scrapethissite.com/pages/simple/ and summarize the page.
```

## Submit to xAI

The [xAI plugin marketplace](https://github.com/xai-org/plugin-marketplace) is
an index; a submission is one pull request that adds a catalog entry and the
regenerated component index. Follow its
[CONTRIBUTING.md](https://github.com/xai-org/plugin-marketplace/blob/main/CONTRIBUTING.md).

1. Pin the release commit:

   ```bash
   git ls-remote https://github.com/mrscraper-com/mrscraper-grok-plugin.git HEAD
   ```

2. Fork `xai-org/plugin-marketplace` and branch from `main`. Append a
   `mrscraper` entry to the `plugins` array in
   `.grok-plugin/marketplace.json`, formatted like the existing entries, with
   the catalog name, description, category, homepage, keywords, and domains
   from the table above and this `source` object:
   `"source": "url"`,
   `"url": "https://github.com/mrscraper-com/mrscraper-grok-plugin.git"`,
   `"path": "plugins/mrscraper"`, and `"sha"` set to the full 40-character
   commit SHA from step 1. Tags, branch names, and abbreviated SHAs are
   rejected.

3. Regenerate the component index and run the checks CI runs:

   ```bash
   python3 scripts/generate-plugin-index.py
   python3 scripts/validate-catalog.py
   python3 scripts/generate-plugin-index.py --check
   ```

4. Open the pull request from an account that visibly belongs to MrScraper,
   fill in the template, and declare the network endpoints and credentials
   from the [README](README.md#data-and-permissions).
5. Reviewers may ask for a short Grok Build demo of sign-in and a harmless
   request. To test pull request number `N`, add
   `branch = "refs/pull/N/merge"` to the existing **xAI Official** source in
   `~/.grok/config.toml` (do not add a second source), restart Grok Build,
   open `/marketplace`, press `r`, and install `mrscraper`:

   ```toml
   [[marketplace.sources]]
   name = "xAI Official"
   git = "https://github.com/xai-org/plugin-marketplace.git"
   branch = "refs/pull/N/merge"
   ```

   Remove the `branch` line afterwards.
6. If `main` moves and `.grok-plugin/plugin-index.json` conflicts, rebase onto
   `main`, take `main`'s copy of the index, and regenerate it.

## Release updates after listing

xAI's daily bump workflow opens a pin-update pull request when this
repository's `HEAD` moves and the version in
`plugins/mrscraper/.grok-plugin/plugin.json` changes. Bump that version for
every release; a commit without a version change is not picked up. To ship an
update sooner, open a pull request that changes only the `sha` and regenerates
the index.
