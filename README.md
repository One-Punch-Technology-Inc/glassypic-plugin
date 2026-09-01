# GlassyPic Plugin

> **GitHub account: `onepunchtechnology`.** company (One Punch Technology) repo. Run `gh auth switch --user onepunchtechnology` once at the start of any session and **stay on it** for all `gh` / `git push` work — do not switch back to a personal account mid-session.

AI-powered image optimization for your coding workflow. Compress, resize, upscale, convert formats, and generate SEO metadata — all from your AI assistant.

Works with **Claude Code**, **Cursor**, and **GitHub Copilot CLI**.

## Installation

### Claude Code

```bash
claude /plugin install https://github.com/onepunchtechnology/glassypic-plugin
```

### GitHub Copilot CLI

```bash
copilot plugin install onepunchtechnology/glassypic-plugin
```

### Cursor

Install from the [Cursor Marketplace](https://cursor.com/marketplace) — search for "GlassyPic".

Or add manually: clone this repo and load as a project plugin.

## Requirements

- **Node.js 18+**
- No signup required — 20 free credits/day as guest

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
| `status` | Check account tier, credits remaining, and reset time |
| `login` | Authenticate via browser for more credits |
| `logout` | Sign out and clear local session |
| `upgrade` | Open pricing page for higher credit limits |

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

## Pricing

| Tier | Credits | Cost |
|------|---------|------|
| Guest | 20/day | Free, no signup |
| Free | 50/day | Free with account |
| Starter | 1,000/month | $10/month |
| Pro | 3,000/month | $29/month |
| Max | 10,000/month | $89/month |
| Enterprise | Custom | [Contact us](mailto:hello@glassypic.com) |

Use the `status` tool to check your remaining credits. Use `login` to authenticate for more credits.

## Links

- [GlassyPic](https://glassypic.com) — Web app
- [npm package](https://www.npmjs.com/package/@glassypic/mcp-server) — Standalone MCP server
- [GitHub](https://github.com/onepunchtechnology/glassypic-plugin) — This repo

## License

MIT
