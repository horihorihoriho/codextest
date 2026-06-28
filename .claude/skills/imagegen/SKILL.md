---
name: imagegen
description: Generate image files from natural-language prompts inside Claude Code. Two modes — (A) OpenAI Images API when OPENAI_API_KEY is set and api.openai.com is reachable, producing photorealistic PNG/JPEG/WebP via gpt-image-2 (released 2026-04-21); (B) native SVG fallback written by Claude Code itself, producing vector illustrations with optional rasterization to PNG. Use when the user needs visual assets saved to disk such as icons, hero illustrations, banners, OG images, infographics, feature cards, or placeholder images. Triggers — "generate image", "create illustration", "make an icon", "create a banner", "OG image", "imagegen", "GPT Image 2", "gpt-image-2", "画像を作って", "イラスト生成", "アイコン作成", "バナー作成", or whenever the user wants a visual file written to the local filesystem. Do NOT use for image analysis or screenshot review.
---

# imagegen — Claude Code image generation (OpenAI + SVG fallback)

## Overview

Generates images and writes them to disk from inside Claude Code. No separate CLI tool required.

Two modes:

- **Mode A — OpenAI Images API (preferred when available).** Uses the OpenAI Images endpoint (`POST https://api.openai.com/v1/images/generations`) with model `gpt-image-2` (released 2026-04-21; current snapshot `gpt-image-2-2026-04-21`). Requires `OPENAI_API_KEY` AND outbound HTTPS access to `api.openai.com`. Org Verification on the OpenAI dashboard may be required before the key can call `gpt-image-2`. Produces photorealistic raster output.
- **Mode B — Native SVG fallback.** Claude writes the SVG directly. Always works, no API key, no network egress needed. Best for icons, flat illustrations, hero graphics, infographics.

The skill **auto-selects** the mode at runtime using the preflight check below. Mode A is only used when both prerequisites pass; otherwise Mode B is used automatically and the chosen mode is reported to the user.

## Preflight: auto-select the mode

Before generating, run this check exactly once per skill invocation:

```bash
imagegen_mode() {
  if [ -z "$OPENAI_API_KEY" ]; then echo "B reason=no-OPENAI_API_KEY"; return; fi
  code=$(curl -sS -o /dev/null -w "%{http_code}" -m 10 \
         -H "Authorization: Bearer $OPENAI_API_KEY" \
         https://api.openai.com/v1/models 2>/dev/null || echo 000)
  case "$code" in
    200) echo "A" ;;
    401|403) echo "B reason=api-rejected-key-or-egress-denied($code)" ;;
    000)     echo "B reason=network-unreachable" ;;
    *)       echo "B reason=unexpected-http-$code" ;;
  esac
}
imagegen_mode
```

Report the chosen mode to the user in one short sentence (e.g. "Using Mode B — SVG fallback (OPENAI_API_KEY not set)").

## Workflow — parse the request

Whichever mode runs, extract these slots:

- **Subject / scene** — what the image depicts
- **Style** — photorealistic / flat / line-art / isometric / minimalist (Mode A supports all, Mode B excludes photorealistic)
- **Use case** — hero illustration, feature card, OG image, favicon, decoration
- **Dimensions** — width × height (defaults: hero 1200×800, card 800×600, OG 1200×630, icon 256×256)
- **Color palette** — pick from user-specified brand colors; infer from context if unspecified
- **Output path** — default `./assets/img/<slug>.<ext>`
- **Output format** — `png` (default for A), `svg` (default for B), `jpeg`, `webp`

Fill missing slots with sensible defaults. Only ask the user when the request is genuinely ambiguous.

## Mode A — OpenAI Images API (gpt-image-2)

### Endpoint and request

```bash
mkdir -p "$(dirname "$OUT")"
# Round W and H to multiples of 16 (gpt-image-2 requirement)
W16=$(( (W + 15) / 16 * 16 ))
H16=$(( (H + 15) / 16 * 16 ))

curl -sS https://api.openai.com/v1/images/generations \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d "$(jq -n --arg p "$PROMPT" --arg s "${W16}x${H16}" --arg f "$FORMAT" \
        '{model:"gpt-image-2", prompt:$p, size:$s, n:1, quality:"high", output_format:$f}')" \
  > /tmp/imagegen_resp.json

# gpt-image-2 always returns base64; decode into the requested file
jq -r '.data[0].b64_json' /tmp/imagegen_resp.json | base64 -d > "$OUT"
ls -la "$OUT"
```

Request parameters for `gpt-image-2`:

| Param | Allowed values | Notes |
|---|---|---|
| `model` | `"gpt-image-2"` | Pin with `"gpt-image-2-2026-04-21"` if you need a stable snapshot. |
| `prompt` | string, up to 32,000 chars | Front-load the first 50 words. |
| `size` | arbitrary `WIDTHxHEIGHT` | Both must be **divisible by 16**. Examples: `1024x1024`, `1536x864`, `1200x624` (round 630→624). |
| `n` | 1–10 | Default 1. |
| `quality` | `"high"` \| `"medium"` \| `"low"` | Default `"high"` for hero/feature work; `"medium"` for batches. |
| `output_format` | `"png"` \| `"jpeg"` \| `"webp"` | Default `"png"`. |
| `background` | `"transparent"` \| `"opaque"` \| `"auto"` | Only meaningful with `png` / `webp`. |
| `output_compression` | 0–100 | Only with `jpeg` / `webp`. |

The response **always** contains base64 in `.data[i].b64_json` for GPT image models — there is no `url` field, regardless of which `response_format` is requested. Don't try `response_format: "url"`.

If the response contains an `error` field, do NOT retry blindly:
- `invalid_api_key` → tell the user the key is bad
- `organization_must_be_verified` → the org needs to complete API Organization Verification in the OpenAI dashboard before `gpt-image-2` can be called
- `rate_limit_exceeded` → wait, then retry once
- `content_policy_violation` → rephrase the prompt
- `model_not_found` → the key/org doesn't have `gpt-image-2` access yet; fall back to `gpt-image-1` (sizes `1024x1024` / `1024x1536` / `1536x1024` only) or to Mode B
- Network 403 from the local agent proxy → the org egress policy blocks `api.openai.com` (see proxy README); fall back to Mode B

### Setting OPENAI_API_KEY in this environment

If `OPENAI_API_KEY` is not set, the user can configure it via Claude Code settings:

```jsonc
// ~/.claude/settings.json (user-level)
{
  "env": { "OPENAI_API_KEY": "sk-..." }
}
```

Or export it before running Claude Code: `export OPENAI_API_KEY=sk-...`.

If `api.openai.com` returns 403 via the agent proxy, the environment's network egress policy denies the host — the user has to widen the policy in their environment configuration (not something the skill can change at runtime). Use Mode B until that's resolved.

## Mode B — native SVG fallback

Compose an SVG mentally before writing:

- **Layout** — viewBox `0 0 W H`; rule of thirds; leave breathing room
- **Shapes** — rect/circle/path/polygon for main elements
- **Gradients & shadows** — `<defs>` with `<linearGradient>`, `<radialGradient>`, `<filter>` for depth
- **Color harmony** — 2–4 main colors plus neutrals; complementary or analogous
- **Accessibility** — include `<title>` and `<desc>`

Required SVG structure:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 W H" width="W" height="H"
     role="img" aria-labelledby="title desc">
  <title id="title">Short title</title>
  <desc id="desc">Longer description.</desc>
  <defs>
    <!-- gradients, filters, patterns -->
  </defs>
  <!-- background → midground → foreground -->
</svg>
```

Rules:
- Always set `viewBox` AND explicit `width`/`height`
- `xmlns="http://www.w3.org/2000/svg"` on root
- Group with `<g>` and meaningful `id`/`class`
- Round numeric values to 1–2 decimals
- Prefer `path` with relative commands for complex shapes

Use the `Write` tool to save the SVG.

### Rasterizing Mode B output to PNG/JPEG/WebP

If raster output is requested, probe these tools in order (first available wins):

```bash
# rsvg-convert (librsvg) — preferred
rsvg-convert -w W -h H -f png -o OUT.png IN.svg

# Inkscape
inkscape IN.svg --export-type=png --export-filename=OUT.png -w W -h H

# ImageMagick
convert -background none -density 192 -resize WxH IN.svg OUT.png

# Python fallback
python3 -c "import cairosvg; cairosvg.svg2png(url='IN.svg', write_to='OUT.png', output_width=W, output_height=H)"
```

If none are available, install one (`apt-get install -y librsvg2-bin` / `brew install librsvg` / `pip install cairosvg`). If installation isn't feasible, deliver the SVG and tell the user.

For JPEG/WebP, rasterize to PNG first, then convert with `convert` / `cwebp` / `cjpeg`.

## Design recipes (Mode B)

**Hero illustration** — soft gradient background, stylized main subject center-right, 2–3 floating decorative elements (circles, dots, abstract shapes). 3-color palette with one accent for the focal point.

**Feature card icon (80–256px)** — single-concept icon, bold simple shapes (circles, rounded rects), one accent color, generous white space. Avoid tiny details that won't read.

**Infographic** — clear geometric shapes (bars, segments, nodes) over a clean background; labels via `<text>`; 2-color brand palette plus neutrals.

**OG / social share (1200×630)** — bold typographic centerpiece via `<text>` using system fonts (`system-ui`, `Inter`, `Noto Sans JP`); strong gradient or solid background; logo/icon in corner; important content within central 1080×510 safe area.

## Verify and report

After writing each file, run `ls -la <path>` to confirm and report path, dimensions, and file size. For batches (e.g. a set of feature-card images), generate sequentially. Do not parallelize Write calls.

## Limitations

- **Mode A blocked here** — this environment's egress proxy denies `api.openai.com` (HTTP 403 from policy). The skill auto-falls back to Mode B.
- **Mode B is illustration, not photo** — for photorealistic content, set `OPENAI_API_KEY` and ensure the environment allows egress to `api.openai.com`.
- **In-image typography in Mode B** is limited to fonts installed on the rendering environment.
