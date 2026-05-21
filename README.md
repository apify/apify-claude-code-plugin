# Apify for Claude Code

Official Apify plugin for Claude Code — adds the Apify MCP server, one `apify` routing agent, and five bundled skills for the main Apify workflows: using existing Actors, building and deploying your own Actors, actorizing existing projects, generating Actor output schemas, and integrating Apify into existing applications.

> **Apify** is a platform of thousands of serverless cloud programs called **Actors** for web scraping, browser automation, and data extraction. Learn more at [apify.com](https://apify.com).

## What you get

| Component | Name | Purpose |
|---|---|---|
| Agent (entry point) | `apify` | Routes every Apify request to the right skill or transport path. **This is the one you should invoke.** |
| MCP server | `apify` (`https://mcp.apify.com/`) | Lets the agent search the Apify Store, fetch Actor details, run Actors, and read the Apify docs. |
| Skill | `apify-actor-development` | Create, debug, and deploy a brand new Apify Actor from scratch. |
| Skill | `apify-actorization` | Convert an existing JS/TS, Python, or CLI project into an Apify Actor. |
| Skill | `apify-generate-output-schema` | Generate `dataset_schema.json` / `output_schema.json` / `key_value_store_schema.json` for an existing Actor. |
| Skill | `apify-sdk-integration` | Add Apify Actor execution to an existing application using the `apify-client` package. |
| Skill | `apify-ultimate-scraper` | CLI-driven data extraction workflow for selecting, configuring, and running pre-built Actors across 15+ platforms. |

## Installation

Add the Apify marketplace and install the plugin:

```bash
/plugin marketplace add apify/apify-claude-code-plugin
/plugin install apify@apify
```

Validate the installation:

```bash
/plugin validate .
```

### Prerequisites

- **Claude Code** CLI installed and authenticated.

## First-run setup

The plugin uses **three setup paths** depending on what you're doing. The `apify` agent will guide you through the right one, but here is the high-level map.

### Path 1 — Using existing Actors through MCP

Uses **OAuth**. The first time the agent calls a tool that needs auth (for example `run-actor` or `get-dataset-items`), Claude Code opens `console.apify.com` in your browser and asks you to sign in. Read-only tools such as `search-actors`, `fetch-actor-details`, `search-apify-docs`, and `fetch-apify-docs` work without auth.

### Path 2 — CLI workflows for Actor development, actorization, or scraper runs

These skills expect the local `apify` CLI to be available. Install it first:

```bash
npm install -g apify-cli
```

For interactive use, authenticate with:

```bash
apify login
```

In headless or CI environments, export an **`APIFY_TOKEN`** instead; the CLI can read it automatically:

```bash
export APIFY_TOKEN="apify_api_xxxxxxxxxxxx"
```

Generate a token at [console.apify.com/settings/integrations](https://console.apify.com/settings/integrations). Don't have an account? [Sign up free](https://console.apify.com/sign-up) — no credit card required.

### Path 3 — SDK integration into an existing application

Uses an **`APIFY_TOKEN`** environment variable with the `apify-client` package or the REST API:

```bash
export APIFY_TOKEN="apify_api_xxxxxxxxxxxx"
```

### Working in headless / SSH environments (no browser)

The MCP OAuth flow needs a browser. If you're running Claude Code over SSH or in any environment without a browser, you have these options:

1. **Authenticate locally first.** Run Claude Code once on your laptop so the OAuth refresh token is stored, then reconnect remotely.
2. **Use the CLI-based skills.** `apify-actor-development`, `apify-actorization`, and `apify-ultimate-scraper` can work in headless environments when the `apify` CLI is installed and `APIFY_TOKEN` is exported.
3. **Use the SDK integration skill.** `apify-sdk-integration` uses `apify-client` and only needs `APIFY_TOKEN`.

## How to use it

Start a Claude Code session and describe what you need. The `apify` agent reads the request, chooses between MCP, CLI, or SDK-based workflows, and dispatches to the right skill or tool.

```
find me 5 well-rated coffee shops in Seattle and export to CSV
build me an Actor that scrapes a sitemap and stores titles
add Apify to this Next.js app so I can run a scraper from /api/scrape
generate output schemas for the Actor in this folder
```

## Components reference

### MCP server

The `apify` MCP server is configured in `.mcp.json` and the agent uses it for Route 1 tasks. It exposes:

- `search-actors` — search the Apify Store by keyword (no auth)
- `fetch-actor-details` — Actor specs, input schema, pricing (no auth)
- `run-actor` — execute an Actor and return results (OAuth)
- `get-dataset-items` — retrieve dataset rows from a previous run (OAuth)
- `search-apify-docs` / `fetch-apify-docs` — Apify documentation lookup

### Bundled scripts

This plugin export does **not** include standalone helper scripts. Instead, the bundled skills ship with markdown references that the agent uses while working:

- `skills/apify-actor-development/references/` — Actor config, schemas, logging, standby mode, and README guidance
- `skills/apify-actorization/references/` — JS/TS, Python, and CLI actorization guides plus schema/output notes
- `skills/apify-ultimate-scraper/references/` — Actor index, gotchas, and workflow playbooks for common scraping use cases

The executable requirements come from the skills themselves: Route 1 uses the MCP server, Route 2 relies on the local `apify` CLI, and Route 3 uses the `apify-client` SDK.

## Troubleshooting

**OAuth browser never opens / hangs.** See the "Working in headless / SSH environments" section above and switch to a CLI- or SDK-based path if needed.

**`apify` CLI not found.** Install it with `npm install -g apify-cli` before using `apify-actor-development`, `apify-actorization`, or `apify-ultimate-scraper`.

**`APIFY_TOKEN` not found.** Export `APIFY_TOKEN` in your shell before starting Claude Code when using headless CLI auth or the `apify-sdk-integration` skill.

**The wrong skill keeps getting picked.** The five bundled skills are marked `user-invocable: false` and are meant to be routed through the `apify` agent. If routing looks wrong, describe the goal more explicitly: use existing Actors, build an Actor, actorize a project, generate output schemas, or integrate Apify into an app.

**`apify` vs `apify-client`** — these are two different npm packages. The `apify` package is the SDK for **building** Actors (used inside an Actor's code, on the Apify platform). The `apify-client` package is the API client for **calling** Actors from your own application. The agent picks the right one for you; if you're installing manually, double-check.

## Resources

- Apify Console — [console.apify.com](https://console.apify.com)
- Apify Store — [apify.com/store](https://apify.com/store)
- Docs (LLM-friendly) — [docs.apify.com/llms.txt](https://docs.apify.com/llms.txt)
- Docs (full) — [docs.apify.com/llms-full.txt](https://docs.apify.com/llms-full.txt)
- Source repo — [github.com/apify/apify-claude-code-plugin](https://github.com/apify/apify-claude-code-plugin)
- Issues / feedback — open an issue on the source repo, or email [support@apify.com](mailto:support@apify.com)

## License

Apache-2.0. See [LICENSE](./LICENSE).