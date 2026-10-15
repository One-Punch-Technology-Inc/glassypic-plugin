# GlassyPic Plugin

AI-powered image optimization for your coding workflow. Compress, resize, upscale, convert formats, and generate SEO metadata — all from your AI assistant.

Works with **Claude Code**, **Cursor**, and **GitHub Copilot CLI**.

## Installation

GlassyPic isn't published to any editor's plugin marketplace — you install it straight from this repository.

### Claude Code

Register this repo as a marketplace, then install the plugin from it:

```bash
/plugin marketplace add One-Punch-Technology-Inc/glassypic-plugin
/plugin install glassypic@glassypic-marketplace
```

For local development, load a clone directly instead:

```bash
git clone https://github.com/One-Punch-Technology-Inc/glassypic-plugin
claude --plugin-dir ./glassypic-plugin
```

### GitHub Copilot CLI

```bash
copilot plugin marketplace add One-Punch-Technology-Inc/glassypic-plugin
copilot plugin install glassypic@glassypic-marketplace
```

### Cursor

Cursor loads local plugins from `~/.cursor/plugins/local/`. Clone the repo into a folder there — use a real directory, not a symlink, which currently fails to load — then reload the window:

```bash
git clone https://github.com/One-Punch-Technology-Inc/glassypic-plugin ~/.cursor/plugins/local/glassypic
```

Run **Developer: Reload Window** from the command palette (Cmd/Ctrl+Shift+P) to activate it.

### Any other MCP client

The plugin is a thin wrapper around the `@glassypic/mcp-server` package. To use the tools directly, add this to your client's MCP config:

```json
{
  "mcpServers": {
    "glassypic": {
      "command": "npx",
      "args": ["-y", "@glassypic/mcp-server@2.1.2"]
    }
  }
}
```

The plugin pins the server to an exact version. Check [npm](https://www.npmjs.com/package/@glassypic/mcp-server) for newer releases.

## What the plugin sends and stores

The plugin runs the `@glassypic/mcp-server` package on your machine through `npx`, which downloads it from npm the first time. The skills themselves only read and edit files in your project.

When a tool runs, the server talks to `https://api.glassypic.com`:

- `optimize_image` uploads the image you point it at (a local file or a URL) for processing, then downloads the result. It writes the result as a new file next to the original, or wherever `output_path` says. It never overwrites the original.
- Every request carries a session token so the API can count your credits. `status` sends only that token.
- `login` opens a sign-in page on `glassypic.com` in your browser. `upgrade` opens the plans page there and makes no API call.
- `logout` asks the API to revoke your sign-in token.

The server keeps its tokens in `~/.glassypic/session.json` (readable only by you): a guest token from your first request, plus your sign-in token once you log in. `logout` removes the sign-in token and leaves the guest one. The server reads no other files except the images you pass to `optimize_image`.

## Requirements

- **Node.js 18+**
- No account required. GlassyPic grants free credits daily to guests and signed-in users alike — run the `status` tool to see what you have.

## Skills

### `optimize-images`

Batch-optimize images in your project. Audits file sizes, selects optimal formats, and compresses everything with a before/after savings report.

```
"Optimize all images in src/assets/"
"Compress the images in this project for production"
"Convert all PNGs to WebP"
```

### `image-seo`

Scan your codebase for images missing alt text and generate AI-powered SEO metadata.

```
"Add alt text to all images missing it"
"Audit image SEO in this project"
"Generate alt text for the product images"
```

### `web-performance-images`

Audit and fix image-related Core Web Vitals issues: LCP hero images, CLS dimensions, and lazy loading.

```
"Fix image performance issues in this project"
"Audit Core Web Vitals for images"
"Which images are slowing down page load?"
```

## MCP Tools

The plugin connects to the GlassyPic MCP server and exposes the following tools:

| Tool | Description |
|------|-------------|
| `optimize_image` | Compress, resize, upscale, convert format, and generate SEO tags |
| `status` | Check credits remaining and reset time |
| `login` | Sign in via your browser |
| `logout` | Sign out and clear the local session |
| `upgrade` | Raise your daily credit limit |

### `optimize_image` Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `input` | string (required) | Absolute file path or URL |
| `output_path` | string | Where to save (file path or directory ending in `/`) |
| `output_format` | enum | `jpg`, `png`, `webp`, `avif`, `gif`, `svg`, `ico`, `original` |
| `output_width_px` | int | Target width in pixels |
| `output_height_px` | int | Target height in pixels |
| `output_upscale_factor` | `2` \| `4` | AI upscale factor |
| `output_resize_behavior` | enum | `pad` or `crop` (when both dimensions set) |
| `output_seo_tag_gen` | boolean | Generate alt text, keywords, and SEO filename (default: `true`) |
| `output_file_size_limit` | int | Target max output file size in bytes |
| `confirm_gif_cost` | boolean | Required to proceed with animated GIF processing after cost warning |
| `gif_frame_limit` | int (1–100) | Max frames to process for animated GIFs (default: `100`) |

**Supported input formats:** JPG, PNG, WebP, AVIF, GIF (animated), HEIC, TIFF, BMP, SVG, ICO — up to 50 MB.

## Links

- [GlassyPic](https://glassypic.com) — Web app
- [npm package](https://www.npmjs.com/package/@glassypic/mcp-server) — Standalone MCP server
- [GitHub](https://github.com/One-Punch-Technology-Inc/glassypic-plugin) — This repo

## License

MIT
