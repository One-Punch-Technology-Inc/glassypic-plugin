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
      "args": ["-y", "@glassypic/mcp-server@2.1.3"]
    }
  }
}
```

The plugin pins the server to an exact version. Check [npm](https://www.npmjs.com/package/@glassypic/mcp-server) for newer releases.

## What the plugin sends and stores

The plugin runs the `@glassypic/mcp-server` package on your machine through `npx`, which downloads it from npm the first time.

The skills tell your assistant to scan your project for images (with Glob, Grep, and shell tools such as `find`, `ls`, `identify`, or `sips`), edit image tags in your code, and call the server's tools. `image-seo` also creates a temporary directory for the optimized copies it doesn't need and deletes it afterwards.

### Network

The server talks to `https://api.glassypic.com`:

- **`optimize_image`, local file:** uploads the file's bytes, then downloads the processed result.
- **`optimize_image`, URL:** sends the URL itself, including any query string such as a signed link, and GlassyPic's servers fetch the image. The image host sees GlassyPic's request, not yours.
- **Credentials:** a guest's very first upload goes out with no token. The response creates a guest session, and later requests send that token in an `X-Session-Token` header. Once you sign in, requests send your sign-in token as `Authorization: Bearer` instead.
- **`login`:** requests a one-time code, opens a sign-in page on `glassypic.com` in your browser, and checks every 3 seconds, for up to 5 minutes, whether you finished. These requests carry no token.
- **`status`:** sends only your token.
- **`logout`:** asks the API to revoke your sign-in token.
- **`upgrade`:** opens the plans page on `glassypic.com` in your browser and makes no API call.

Uploaded images and their processed versions are kept for a period that depends on your account tier, then deleted. The [privacy policy](https://glassypic.com/privacy/) has the schedule.

Setting the `GLASSYPIC_API_URL` environment variable sends all of the above, images and tokens included, to that host instead (HTTPS required, except on localhost). The plugin doesn't set it.

### Files on your machine

- **Images:** reads the local images you pass to `optimize_image`. Writes each result as a new file and never overwrites the original:
  - with `output_path` set, the file goes there
  - for a local file without it, the file goes next to the original
  - for a URL without it, the file goes in the server's working directory
- **Session file:** a file in `~/.glassypic/`, readable only by you. It holds your guest token, plus your sign-in token, email address, and account tier once you log in. `logout` removes the sign-in token, email, and tier and keeps the guest token.

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
