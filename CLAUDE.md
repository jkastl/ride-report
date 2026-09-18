# turbulence

Single-file mobile device motion tracker for iOS Safari and Android Chrome.

## Overview

`index.html` — the entire app. No build step, no dependencies.

Hosted on GitHub Pages from the `main` branch root.

## What it does

- Requests `DeviceMotionEvent` permission (required on iOS 13+)
- Reads `acceleration` (gravity-excluded) from the motion sensor
- Dead-reckons an X/Y position on the canvas, as if the phone is lying flat on a table
  - X axis: left ↔ right
  - Y axis: forward ↔ back
  - Z axis: vertical (shown in readout only)
- Falls back to `accelerationIncludingGravity` with a gravity high-pass when `acceleration` is unavailable
- Draws a color-gradient trail (blue=old → red=current) with a pulsing dot at the head
- Optional non-linear radial scale (`Curve`) so small bumps use more of the canvas
- Live numeric readout at the bottom (`Raw #s`)
- Clear, Pause, Smooth, Invert, Curve, Awake (screen wake lock) and Raw #s controls

## Development

Edit `index.html` directly. Test on a real device (simulator won't fire motion events).

```
open index.html   # desktop preview (no motion data)
```

Push to `main` to deploy via GitHub Pages.

## Instructions for Claude

- Just build the feature. No tests, no code analysis, no linting, no refactoring suggestions, no comments explaining what code does.
- Bump the minor version in `index.html` (`v1.x`) and update the date (`YYYY-MM-DD`) with every change.
- Single file only — keep everything in `index.html`.
