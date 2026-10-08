# Changelog

All notable changes to the **Apify for Claude Code** plugin are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.2]

### Added
- Anthropic directory listing fields in `.claude-plugin/plugin.json`: `displayName`, `icon`, `documentationUrl`, `supportUrl`, `privacyPolicyUrl`, and `termsOfServiceUrl`.
- Plugin icon for the directory listing.
- `author.email` in `.claude-plugin/plugin.json`.

### Changed
- Updated `keywords` to `apify`, `data`, `scraping`, `web-scraping`, `automation`.
- Bumped `version` to `1.0.2` in `plugin.json` and `marketplace.json`.

## [1.0.1]

### Changed
- Updated Apify MCP server URL in `.mcp.json` to include `?client=claude+code+plugin` for client identification.

## [1.0.0] — Initial Claude Code release

### Added
- `apify` MCP server entry in `.mcp.json` pointing to `https://mcp.apify.com/`.
- `.claude-plugin/plugin.json` manifest with plugin metadata.
- Apache-2.0 license.