# Changelog

## 2.0.0 (2026-09-01)

### Changed

- **Renamed from `tinify-ai` to `glassypic`** across all platform manifests. This is a breaking change.
- MCP server package is now `@glassypic/mcp-server` (was `@tinify-ai/mcp-server`, deprecated on npm)
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
