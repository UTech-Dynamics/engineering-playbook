# Image Format Selection: WebP vs JPG vs PNG

> **Scope:** Engineering guidance for choosing raster image formats on the web, with emphasis on rendering speed, transparency, compression behavior, and production delivery patterns.

## TL;DR

Use **WebP** as the default for most web images: it delivers 25–35% smaller files than JPG at equivalent perceptual quality and 10–26% smaller files than PNG in lossless mode, while supporting full alpha-channel transparency[reference:0][reference:1]. Keep **JPG** as the universal fallback for photographs and legacy environments. Reserve **PNG** for lossless precision (diagrams, screenshots with sharp text, pixel-art) and for transparency in legacy pipelines that cannot produce WebP[reference:2][reference:3].

| Format | Compression | Transparency | Decode speed | Browser support (2026) | Best for |
|---|---|---|---|---|---|
| **WebP** | Lossy + lossless | Yes (alpha) | Fast (slightly slower than JPG on some devices) | ~97% | Photos, UI graphics, transparent assets — default choice |
| **JPG** | Lossy only | **No** | Fastest | 100% | Photographs, legacy fallback, scenarios where encode speed matters |
| **PNG** | Lossless only | Yes (full alpha) | Fast on simple images; can be slower at high resolutions | 100% | Diagrams, screenshots, pixel-art, text-heavy UI, lossless archival |

---

## 1. Rendering Speed: What “Speed” Actually Means

Rendering speed for images is not a single metric. It decomposes into:

1. **Transfer time** — network bytes / bandwidth.
2. **Decode time** — CPU cycles to turn compressed bytes into pixels.
3. **Paint/composite time** — GPU/browser work to place the image on screen.

Format choice affects all three, sometimes in opposing directions.

### 1.1 Decode speed benchmarks

Controlled measurements on mobile-class CPUs show the following decode times for a photographic image at web-optimized settings[reference:4]:

| Format | Decode time (mobile CPU) | Relative size |
|---|---|---|
| JPG (q85) | **12 ms** | Baseline |
| PNG-24 | **8 ms** | 29% larger than JPG |
| WebP (q80) | **18 ms** | 67.5% smaller than JPG |

**Interpretation:** PNG decodes fastest on this particular workload because lossless DEFLATE decompression is cheap, but the file is dramatically larger. WebP’s decode is slightly slower than JPG — often in the 10–30 ms range on modern hardware — but the transfer-time savings from smaller files dominate in real network conditions[reference:5]. On a 4G connection, WebP has been measured about **126 ms faster to load** than JPG for the same perceptual quality because the byte savings outweigh the decode overhead[reference:6].

> **Engineering rule:** Optimize for **total load time (transfer + decode + paint)**, not decode time alone. On slow networks, the smallest acceptable file usually wins. On CPU-constrained low-end devices, JPG or PNG may close the gap — test on your actual device matrix.

### 1.2 Encode speed (build-time consideration)

Encode speed matters for CI pipelines and user-upload paths, not runtime rendering:

- **JPG** encodes fastest (≈0.05 s for a 12 MP photo on one core).
- **WebP** is roughly **2–3× slower than JPG** but still fast (≈0.18 s for the same image)[reference:7][reference:8].
- **PNG** lossless compression is comparatively expensive at encode time and produces large files.

If you generate derivatives in a request path, WebP is usually acceptable; JPG is the safest for synchronous, latency-sensitive encoding.

---

## 2. Transparency and Alpha Channels

Transparency is the decisive constraint for many UI assets, and it eliminates JPG outright.

### 2.1 Support matrix

| Format | Transparency support | Alpha quality | Notes |
|---|---|---|---|
| **JPG** | **None** | — | JPEG has no alpha channel; transparent areas become white or black depending on the encoder[reference:9]. |
| **PNG** | Full alpha (PNG-24: 8-bit alpha, 256 opacity levels per pixel) | Lossless, crisp edges | The historical standard for logos, icons, and overlays requiring smooth anti-aliased edges[reference:10]. |
| **WebP** | Full alpha in **both lossy and lossless modes** | Lossy WebP can introduce subtle artifacts in fine alpha edges | Lossless WebP adds alpha at only **~22% additional bytes** over lossless RGB; lossy WebP with alpha is typically **~3× smaller than PNG** for the same transparent content[reference:11]. |

### 2.2 Practical implications

- **Logos, icons, favicons, UI overlays with transparency:** WebP is the modern default. Use PNG only when WebP cannot be produced or when targeting legacy systems that require PNG/ICO. For small favicons (16×16, 32×32), PNG or ICO remains the safe interoperable choice[reference:12].
- **Photographs with transparency (cut-out product shots):** WebP lossy with alpha is usually the best size/quality trade-off. PNG-24 preserves exact edges but at 3–5× the byte cost.
- **Never use JPG when transparency is required.** The format cannot represent it; alpha will be flattened, usually against white, producing visible artifacts on non-white backgrounds.

### 2.3 How browsers composite transparency

Browsers handle transparency via **alpha compositing**: each pixel’s alpha value (0–255) controls how much of the background shows through. For partially transparent edge pixels, the browser computes a blend of foreground and background color. This happens at high speed in the rendering pipeline, but it means that **lossy alpha compression (as in lossy WebP) can produce halos or fringing** if the alpha channel is aggressively quantized[reference:13]. For critical brand assets with soft shadows or fine anti-aliasing, verify visual quality at 100% zoom and against multiple background colors.

---

## 3. Compression Efficiency and File Size

### 3.1 WebP vs JPG

At visually equivalent quality, lossy WebP is consistently **25–35% smaller than JPG**[reference:14][reference:15]. A large-scale test of 500 images found average sizes of **82 KB for WebP (q80)** versus **145 KB for JPG (q85)** for photographs — a 43% reduction[reference:16]. Google’s own comparison using the SSIM quality index reports **25–34% smaller** lossy WebP files than JPG at matched perceptual quality[reference:17].

### 3.2 WebP vs PNG

Lossless WebP files are **~26% smaller than PNG** in Google’s benchmarking[reference:18]. Other 2026 measurements report 10–25% smaller lossless WebP for UI screenshots and icons[reference:19]. The savings depend heavily on content: PNG’s DEFLATE compression performs poorly on photographic content, while WebP’s lossless mode uses more sophisticated prediction and palette techniques.

For lossless screenshots at 1440×900: WebP measured **180 KB** versus PNG at **245 KB** — a 26.5% reduction[reference:20].

### 3.3 PNG for photographs is an anti-pattern

A 1920×1080 photograph measured **3.8 MB as lossless PNG** versus **234 KB as WebP q80** — a ~16× difference[reference:21]. Serving PNG for photographic content inflates payload size by an order of magnitude and directly harms Largest Contentful Paint (LCP). PNG should not be used for photographs on the web.

---

## 4. When to Use Each Format

### 4.1 Decision flowchart (text form)
Does the image need transparency?
├── YES → Is the asset a small favicon / legacy-required icon?
│ ├── YES → PNG (or ICO)
│ └── NO → WebP (lossy for photos with alpha; lossless for graphics)
└── NO → Is the content a photograph or continuous-tone image?
├── YES → WebP (primary) with JPG fallback
└── NO → Is lossless precision required (sharp text, diagrams, pixel-art)?
├── YES → PNG (or lossless WebP if pipeline supports it)
└── NO → WebP (lossy) or JPG for maximum compatibility

### 4.2 Format-specific guidance

**WebP — the default for modern web**
- Photographs, hero images, product shots: lossy WebP at quality 75–82.
- Transparent UI graphics, logos, icons: WebP (lossless for crisp edges; lossy-with-alpha for size-sensitive cases).
- Screenshots and diagrams: lossless WebP when the toolchain supports it; otherwise PNG.
- Browser support is ~97% globally in 2026 and natively supported in all current major browsers[reference:22][reference:23]. A `<picture>` fallback to JPG/PNG covers the remaining 3%.

**JPG — the universal fallback, still fastest to decode**
- Primary format only when the target audience includes a non-trivial share of WebP-incapable browsers or legacy clients.
- Best fallback in `<picture>` chains because of 100% compatibility and fast decoding.
- Never use for transparency, sharp text, or line art.
- Quality 80–85 is the standard web sweet spot.

**PNG — lossless precision and legacy transparency**
- Diagrams, technical schematics, electrical/wiring diagrams where lossy compression blurs high-contrast lines[reference:24].
- Screenshots of code or UI text: lossy formats cause color bleeding and anti-aliasing artifacts around monospaced text[reference:25].
- Pixel-art and assets requiring mathematically exact RGB values.
- Favicons and OS shortcuts where PNG/ICO compatibility is required[reference:26].
- **Do not use for photographs on the web.**

---

## 5. Production Delivery Pattern: The `<picture>` Element

The most robust pattern is to serve the smallest format the browser can decode, with a universal fallback. The `<picture>` element enables content negotiation without JavaScript:
```html
<picture>
  <!-- Primary: WebP for ~97% of traffic -->
  <source srcset="/hero.webp" type="image/webp">

  <!-- Fallback: JPG for legacy browsers and WebP-incapable clients -->
  <img src="/hero.jpg"
       alt="Descriptive alternative text"
       width="1200" height="630"
       loading="lazy"
       decoding="async">
</picture>
```

For transparent assets, the fallback must also support transparency:
```html
<picture>
  <source srcset="/logo.webp" type="image/webp">
  <img src="/logo.png" alt="Company logo" width="200" height="60">
</picture>
```
Engineering notes:

- Always include explicit width and height attributes (or CSS aspect-ratio) to prevent layout shift (CLS).

- Use loading="lazy" for below-the-fold images; avoid lazy-loading LCP hero images.

- For CDNs that support Accept-header content negotiation, the same result can be achieved without <picture> markup by serving WebP when the request advertises image/webp. This is more efficient at scale but requires CDN configuration.

## 6. Quality and Verification

Format benchmarks are directional, not universal. Before locking in quality settings:
1. Encode at the delivery size, not at source resolution. A 4000px source compressed to 1200px behaves differently from a 1200px source.

2. Inspect at 100% zoom on a representative display. Pay attention to:

  - Fine text (JPG and lossy WebP introduce ringing around high-contrast edges).

  - Smooth gradients (lossy formats may band; WebP is generally better than JPG but worse than lossless).

  - Alpha edges against both light and dark backgrounds.

3. Test on the lowest-end device in your support matrix if decode time is a concern. WebP’s decode overhead is negligible on modern phones but can be measurable on very old hardware.

4. Automate fallback verification in CI: confirm that every <picture> block has a valid fallback whose format supports the same feature set (especially transparency).

## 7. Summary: Selection Matrix

```
| Scenario | Primary format | Fallback | Quality setting |
|---|---|---|---|
| Photograph, no transparency | **WebP** (lossy) | JPG | WebP q75–82; JPG q80–85 |
| Photograph with cut-out transparency | **WebP** (lossy + alpha) | PNG-24 | WebP q75–82 |
| Logo / icon with transparency | **WebP** (lossless or lossy+alpha) | PNG-24 | Lossless, or lossy q85+ |
| Favicon (16–32 px) | **PNG** or ICO | — | N/A |
| UI screenshot / code screenshot | **PNG** or lossless WebP | PNG | N/A |
| Diagram / line art / pixel-art | **PNG** or lossless WebP | PNG | N/A |
| Legacy-only environment | **JPG** (photos) / **PNG** (graphics) | — | JPG q80–85 |
```
</hr>

## Default stance for new web projects in 2026:

**WebP first, JPG as the universal photographic fallback, PNG reserved for lossless precision and legacy transparency requirements.** 
This ordering maximizes byte savings and LCP performance while maintaining 100% compatibility through `<picture>`.
