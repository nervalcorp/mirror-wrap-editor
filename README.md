# MirrorWrap Editor

A GPU-accelerated, browser-based image editor that creates seamless mirror-wrap fills around any image. Built with Three.js and custom GLSL shaders — no server, no dependencies, runs entirely in your browser.

![License](https://img.shields.io/badge/license-MIT-blue.svg)
![Built With](https://img.shields.io/badge/built%20with-Three.js%20r128-black.svg)
![Zero Dependencies](https://img.shields.io/badge/dependencies-0-brightgreen.svg)

**[▶ Live Demo](https://nervalcorp.github.io/mirror-wrap-editor/)**

---

## Author

**John L.** — [@nervalcorp](https://github.com/nervalcorp)

---

## Overview

MirrorWrap Editor solves a common problem in print and digital design: you have an image that doesn't match your target canvas dimensions, and you need to fill the remaining space without stretching, cropping out important content, or leaving blank areas.

Instead of stretching or tiling, MirrorWrap reflects the image outward from its edges — creating a seamless, natural-looking extension that preserves the original content at its correct aspect ratio. The mirror fill is computed in real-time on the GPU via a custom fragment shader, so it's fast enough to preview interactively even on large canvases.

### What it does

1. You upload any image (JPG, PNG, WebP, BMP, GIF).
2. The image is placed on a canvas at its natural aspect ratio (contain fit).
3. Any empty space around the image is filled with mirrored reflections of the image edges.
4. You adjust the canvas size, image position, mirror depth, and other parameters.
5. You export at your desired resolution and format — the output is a flat image with the mirror fill baked in.

---

## Features

### Core Mirror System

- **Custom GLSL fragment shader** handles a 9-zone mirror system: the image center samples normally, the 4 sides reflect across the nearest edge, and the 4 corners reflect on both axes — producing seamless, distortion-free tiling.
- **Per-side mirror toggles** — enable or disable mirroring independently for Top, Bottom, Left, and Right. Disabled regions render as transparent (checkerboard). Corner zones follow the intersection of their two adjacent sides.
- **Mirror depth control** with three modes:
  - **Infinite** — mirrors fill the entire canvas (classic behavior).
  - **Fixed** — set depth in pixels, inches, or centimeters. Default: 3 inches at the current DPI.
  - **Proportional** — depth as a percentage of the image's short side.
- **Edge fade** — toggle between a **hard cutoff** (mirror stops abruptly) and a **soft fade** (gradual transparency at the depth boundary). Adjustable fade percentage.

### Canvas & Units

- **Canvas size** in pixels, inches, or centimeters, with a configurable DPI setting (default 300 for print).
- **Aspect ratio presets**: 16:9, 1:1, 4:3, 9:16, 21:9, 4:5, 3:2.
- **Draggable canvas edges** — 8 interactive handles (4 edges + 4 corners) for resizing the canvas visually by dragging.
- All internal math is pixel-based; units are a display layer, so no precision is lost when switching.

### Image Positioning & Fit

- **Contain fit** — the image always displays at its correct aspect ratio, never stretched.
- **Scale slider** (10%–500%) — controls how large the image appears within the canvas.
- **Drag to reposition** — click and drag on the canvas to move the image within it. The mirror fill recalculates dynamically.
- **9-point alignment grid** — snap the image to any anchor: top-left, top-center, top-right, center-left, center, center-right, bottom-left, bottom-center, bottom-right.
- **Soft drag constraint** — the image must always overlap at least 20% with the canvas. You can push it toward edges but never fully off-canvas.

### Crop Tool

- **Non-destructive crop** — click "Crop" to enter crop mode, which overlays the full original image with a draggable/resizable crop rectangle.
- 8 resize handles (corners + edges) plus move-by-dragging.
- Dimmed area outside the crop shows what will be removed.
- "Apply Crop" creates a new texture from the cropped region; "Reset Crop" restores the original at any time.
- The original image is always kept in memory — cropping is reversible.

### Image Boundary Guide

- A dashed amber outline shows exactly where the original image sits within the canvas — the boundary between real content and mirror-filled area.
- Pure CSS overlay, never included in exports.
- Toggle on/off with the eye icon.
- Displays original image dimensions as a label.

### Transform & Adjustments

- **Rotation** (-180° to 180°) — rotates the image within the mirror system; the fill recalculates around the rotated content.
- **Brightness** and **Contrast** — GPU-accelerated via GLSL uniforms. Real-time preview.

### Export

- **Formats**: PNG (lossless), JPEG, WebP — with a quality slider for lossy formats.
- **Resolution scaling**: 10% to 300% of canvas size. The preview is viewport-sized; the export renders offscreen at the exact target resolution via a `WebGLRenderTarget`.
- **Pixel-perfect output** — the export reads pixels from the GPU render target, flips Y, and writes to a downloadable blob. What you see is what you get.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Rendering | Three.js r128 (WebGL) |
| Shader | Custom GLSL (vertex + fragment) |
| Camera | OrthographicCamera (no perspective distortion) |
| UI | Vanilla HTML/CSS/JS (no framework) |
| Fonts | DM Sans + JetBrains Mono (Google Fonts) |
| Export | WebGLRenderTarget → readPixels → Canvas2D → Blob |

Zero npm dependencies. Zero build step. One HTML file.

---

## Getting Started

### Option 1: Just open it

Download `index.html` and open it in any modern browser. That's it.

### Option 2: Serve locally

```bash
git clone https://github.com/nervalcorp/mirror-wrap-editor.git
cd mirror-wrap-editor

# Any static server works
python3 -m http.server 8000
# or
npx serve .
```

Then open `http://localhost:8000` in your browser.

### Option 3: Deploy

Since it's a single HTML file with no build step, you can deploy it anywhere that serves static files: GitHub Pages, Netlify, Vercel, S3, or just drop it on any web server.

---

## Usage

1. **Upload** — click "Upload Image" or drag and drop onto the canvas.
2. **Resize canvas** — type dimensions, pick a preset, or drag the canvas edges.
3. **Position** — drag the image to reposition, use the alignment grid, or adjust scale.
4. **Crop** — click Crop to trim unwanted edges before mirroring.
5. **Mirror settings** — toggle sides, set depth mode, adjust fade.
6. **Transform** — rotate, adjust brightness/contrast.
7. **Export** — pick format, quality, resolution, and hit Save Image.

---

## Browser Support

Requires WebGL 1.0+ support. Tested on:

- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

---

## License

MIT License — see [LICENSE](LICENSE) for details.

---

## Contributing

Contributions are welcome. Please open an issue first to discuss what you'd like to change.
