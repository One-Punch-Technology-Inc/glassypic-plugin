# Changelog

## Unreleased

### Changed

- README installation instructions rewritten to match how each host actually installs an
  unlisted third-party plugin: `plugin marketplace add` from this repo for Claude Code and
  GitHub Copilot CLI, a local clone under `~/.cursor/plugins/local/` for Cursor, plus a raw
  MCP config for any other client.
- Removed the incorrect "install from the Cursor Marketplace" instruction — the plugin is not
  published to any marketplace.
- Consolidated the plugin-marketplace manifest to `.claude-plugin/marketplace.json` (read by
  both Claude Code and Copilot CLI); removed `.github/plugin/marketplace.json`.
- Softened credit-limit language in the three skills: guests are pointed to `login` and
  everyone to the daily reset, instead of "suggest `upgrade`".
- Moved the GitHub-account / contributor guidance out of the README into `AGENTS.md`.

### Added

- `AGENTS.md` — agent/contributor notes (account switching, manifest sync, no-pricing rule);
  `CLAUDE.md` imports it so there is one source of truth.
- `.github/workflows/validate.yml` — CI that checks manifest JSON, cross-manifest name/version
  agreement, and `claude plugin validate`.

### Removed

- Pricing table and paid-tier / pricing-page references from the README. Credits are now a
  one-line note: free daily credits for guests and signed-in users.

## 2.0.0 (2026-09-01)

### Changed

- **Renamed from `tinify-ai` to `glassypic`** across all platform manifests. This is a breaking change.
- MCP server package is now `@glassypic/mcp-server`. The previous package was deprecated on npm
  in June 2026 and unpublished on 2026-09-01; it no longer resolves, so the old install command
  will fail rather than install an outdated version.
- MCP server key in `.mcp.json` is now `glassypic`, so tool identifiers change from
  `mcp__tinify-ai__*` to `mcp__glassypic__*`
- Homepage and contact moved to `glassypic.com`
- Repository moved to `onepunchtechnology/glassypic-plugin`

### Removed

- `assets/logo.png` (old wordmark), and the `logo` field from the Claude Code manifest

### Migration

Reinstall the plugin from the new repository URL. Any project config or custom
prompt that references `mcp__tinify-ai__*` tool names must be updated to
`mcp__glassypic__*`.

## 1.0.0 (2026-03-22)

### Added

- Claude Code manifest (`.claude-plugin/plugin.json`)
- Cursor manifest (`.cursor-plugin/plugin.json`)
- MCP server config (`.mcp.json`) using `@tinify-ai/mcp-server` via stdio
- Skill: `optimize-images` — batch image optimization workflow
- Skill: `image-seo` — image alt text and SEO metadata generation
- Skill: `web-performance-images` — Core Web Vitals image audit
- Rule: `image-best-practices` — always-on image format guidance
