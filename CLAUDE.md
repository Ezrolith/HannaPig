# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is **HannaPig Snake** — a pig-themed HTML5 Canvas Snake game with two variants:
- `iOS Game/index.html` — PWA version with mobile D-Pad controls, touch/swipe support, Service Worker caching, and a phone UI frame mockup
- `snake.html` — Standalone desktop version with keyboard-only controls

There is no build system, package manager, or external dependencies. All code is vanilla HTML/CSS/JavaScript embedded in single HTML files.

## Running the Game

Open either file directly in a browser, or serve via any static HTTP server (required for Service Worker registration in the PWA version):

```bash
# Python
python -m http.server 8000

# Node.js
npx serve .
```

Then navigate to `http://localhost:8000/iOS Game/` or `http://localhost:8000/snake.html`.

The PWA version's Service Worker (`iOS Game/sw.js`) will only activate over HTTPS or localhost.

## Architecture

Both versions are entirely self-contained single-file apps. All CSS, JavaScript, and HTML are inline.

**Game constants (both files):**
- Grid: 15×15 cells on a 300×300px canvas
- Game states: `start`, `playing`, `gameover`
- Speed levels: 6 tiers from 150ms (slow) to 60ms (fast) based on score
- High score persisted in `localStorage`

**Core game loop structure:**
1. `gameLoop()` / `update()` — advances snake position, checks collisions, places food
2. `draw()` — renders canvas frame: checkerboard background, mud splats, food (pulsing apple), snake (pig head with directional ears/eyes + body segments + curly tail)
3. Input handlers — keyboard (`keydown`), touch swipe (`touchstart`/`touchend`), and D-Pad buttons (PWA version only)

**Rendering:** The pig head rotates based on movement direction. Body segments alternate shading. Food pulses using a sine wave on `Date.now()`.

**PWA-specific files (`iOS Game/`):**
- `manifest.json` — app name, theme color (#e88da5), display mode standalone
- `sw.js` — cache-first strategy, cache key `hannapig-v1`, caches index.html + manifest.json + icon.svg
- `icon.svg` — inline SVG pig emoji

## Key Differences Between Versions

| Feature | `iOS Game/index.html` | `snake.html` |
|---|---|---|
| Mobile D-Pad | Yes | No |
| Touch swipe | Yes | No |
| Service Worker | Yes | No |
| Phone UI frame | Yes | No |
| Controls | Keyboard + touch + D-Pad | Keyboard only |
