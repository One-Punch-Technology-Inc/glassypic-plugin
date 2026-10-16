# Changelog

## Unreleased

### Added

- `.claude-plugin/plugin.json` sets `documentationUrl`, `supportUrl`, `privacyPolicyUrl`, and
  `termsOfServiceUrl` for the Claude plugin directory listing. Claude Code ignores them.

## 2.1.1 (2026-10-16)

### Changed

- `.mcp.json` and the README's MCP config example pin `@glassypic/mcp-server@2.1.3`. The server
  no longer states credit allowances in its tool text; `status` reports the live numbers.

## 2.1.0 (2026-10-15)

### Changed

- `.mcp.json` pins the MCP server to `@glassypic/mcp-server@2.1.2` instead of resolving
  whatever is latest on npm. The Claude plugin directory blocks unpinned `npx` launchers, and
  pinning keeps every install on a tested server. Each new server release now needs a plugin
  release.
- Skills no longer leave stray image files in the user's project. `image-seo` writes the
  optimized copies to a temp directory. `web-performance-images` turns off SEO tagging when it
  compresses or resizes (the tag renamed the output, which broke references) and reads image
  dimensions with local tools instead of a paid `optimize_image` call. `optimize-images` asks
  before it replaces originals or updates references.

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

- Repository moved to the `One-Punch-Technology-Inc` organization:
  `https://github.com/One-Punch-Technology-Inc/glassypic-plugin`. Manifests, README install
  commands, and `AGENTS.md` point there now. The old `onepunchtechnology/glassypic-plugin`
  URL redirects, so existing marketplace installs keep working.

### Added

- README section "What the plugin sends and stores": what goes to `api.glassypic.com`, what
  opens in the browser, and what's kept in `~/.glassypic/`.
- `AGENTS.md` — agent/contributor notes (account switching, manifest sync, no-pricing rule);
  `CLAUDE.md` imports it so there is one source of truth.
- `.github/workflows/validate.yml` — CI that checks manifest JSON, cross-manifest name/version
  agreement, and `claude plugin validate`.

### Removed

- Pricing table and paid-tier / pricing-page references from the README. Credits are now a
  one-line note: free daily credits for guests and signed-in users.

## 2.0.0 (2026-09-01)

### Changed

- **Renamed the plugin to `glassypic`** across all platform manifests. This is a breaking change.
- MCP server package is now `@glassypic/mcp-server`. The previous package was deprecated on npm
  in June 2026 and unpublished on 2026-09-01; it no longer resolves, so the old install command
  will fail rather than install an outdated version.
- MCP server key in `.mcp.json` is now `glassypic`, so tool identifiers are now
  `mcp__glassypic__*`
- Homepage and contact moved to `glassypic.com`
- Repository moved to `onepunchtechnology/glassypic-plugin`

### Removed

- `assets/logo.png` (old wordmark), and the `logo` field from the Claude Code manifest

### Migration

Reinstall the plugin from the new repository URL. Update any project config or
custom prompt that uses the old tool prefix to `mcp__glassypic__*`.

## 1.0.0 (2026-03-22)

### Added

- Claude Code manifest (`.claude-plugin/plugin.json`)
- Cursor manifest (`.cursor-plugin/plugin.json`)
- MCP server config (`.mcp.json`) running the MCP server via stdio
- Skill: `optimize-images` — batch image optimization workflow
- Skill: `image-seo` — image alt text and SEO metadata generation
- Skill: `web-performance-images` — Core Web Vitals image audit
- Rule: `image-best-practices` — always-on image format guidance
