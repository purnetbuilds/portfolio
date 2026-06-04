# Pixel Cube Cursor Trail — Design Spec
**Date:** 2026-04-29  
**Status:** Approved for implementation

---

## Context

The existing cursor effect (a small dot + lagging dashed ring) is functional but visually minimal. The goal is a loud, expressive cursor trail that reflects the creative personality of the portfolio — vibrant pixel cubes that spawn and fade as the cursor moves, inspired by retro pixel art and neon gradient aesthetics.

---

## Overview

A canvas-based particle system renders mixed-size gradient pixel cubes around the cursor. Cubes spawn on mouse movement, drift slightly, and fade out over ~0.8 seconds. The effect replaces the cursor ring but keeps the cursor dot for precision.

---

## Architecture

### HTML
Add a single canvas element immediately after the existing cursor elements:
```html
<canvas id="pixelCanvas"></canvas>
```

### CSS
```css
#pixelCanvas {
  position: fixed;
  top: 0; left: 0;
  width: 100%; height: 100%;
  pointer-events: none;
  z-index: 9999;
}
```
Remove `.cursor-ring` and `#cursorRing` CSS/HTML (ring is replaced by the pixel cloud).

### JavaScript — Particle System (~60 lines)

**Initialization:**
- Get canvas context, set canvas dimensions to `window.innerWidth/Height`
- Resize canvas on `window.resize`

**Particle object shape:**
```js
{
  x, y,          // spawn position + random scatter ±20px
  vx, vy,        // random drift ±1.5px/frame
  size,          // random 6–28px
  rotation,      // random 0–45° (static, not spinning)
  alpha,         // starts 0.85
  color          // picked from palette (see below)
}
```

**Spawn logic (on `mousemove`):**
- Spawn 2–4 cubes per event
- Position: `cursor ± random(0, 20)` on both axes
- Throttle: skip spawn if last event < 8ms ago (prevents burst on fast scroll)

**Update loop (`requestAnimationFrame`):**
1. Clear canvas with `clearRect`
2. For each particle:
   - `x += vx`, `y += vy`
   - `alpha -= 0.018`
   - Remove if `alpha <= 0`
3. Draw each cube:
   - `ctx.save()` → translate to center → rotate → draw rect from `-size/2`
   - Fill with `createLinearGradient` (color → lighter tint, 2 stops)
   - `ctx.restore()`
4. If particle array empty and mouse idle 2s+, cancel RAF (resume on next move)

**Cap:** Maximum 120 particles in array. If at cap, skip spawning new ones.

---

## Color Palette

Portfolio base colors + vibrant pops, evenly weighted:

| Swatch | Hex | Role |
|--------|-----|------|
| Purple | `#7C3AED` | Base |
| Indigo | `#6366F1` | Base |
| Violet | `#8B5CF6` | Base |
| Rose | `#F43F5E` | Pop |
| Cyan | `#06B6D4` | Pop |
| Lime | `#84CC16` | Pop |
| Amber | `#F59E0B` | Pop |

Each cube gradient: `color` (100% alpha) → `color` lightened by 40% (0% alpha at end stop) — creates an inner glow effect.

---

## Cursor Ring Removal

The `.cursor-ring` element and all its CSS (`cursor-hover`, `cursor-dark` ring states) are removed. The `.cursor-dot` is kept. The pixel cloud provides the visual mass the ring used to.

Hover state on interactive elements: instead of ring expanding, spawn a small burst of 6–8 cubes on `mouseenter`.

---

## Files Modified

| File | Change |
|------|--------|
| `index.html` | Remove ring HTML/CSS, add canvas + pixel JS |
| `contact.html` | Same (contact page has its own cursor setup) |
| `build-lab.html` | Same |

---

## Performance

- Particle array capped at 120
- RAF paused when array is empty + mouse idle >2s
- Canvas sized once on load + on resize (no per-frame size check)
- `pointer-events: none` on canvas (zero interaction overhead)
- No external dependencies

---

## Verification

1. Start dev server: `node server.js`
2. Open `http://localhost:3000`
3. Move cursor — cubes should spawn, drift, and fade
4. Move fast — trail should feel dense without stutter
5. Hover over cards/links — small burst fires on mouseenter
6. Idle 2+ seconds — RAF pauses (check via DevTools performance panel)
7. Resize window — canvas resizes correctly, no offset
8. Check `contact.html` and `build-lab.html` — same effect present
9. Mobile: effect disabled (no cursor on touch devices — guard with `matchMedia('(pointer: fine)')`)
