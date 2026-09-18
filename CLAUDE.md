# ride-report

Ride Report — single-file mobile ride-quality meter for iOS Safari and Android Chrome.

## Overview

`index.html` — the entire app. No build step, no dependencies.

Hosted on GitHub Pages from the `main` branch root.

## What it does

- Requests `DeviceMotionEvent` permission (required on iOS 13+)
- Reads `acceleration` (gravity-excluded) from the motion sensor
- Plots acceleration directly on the canvas (no integration), as if the phone is lying flat on a table
  - X axis: left ↔ right
  - Y axis: forward ↔ back
  - Z axis: vertical (bar on the right edge, plus the readout)
- Falls back to `accelerationIncludingGravity` with a gravity high-pass when `acceleration` is unavailable
- Draws a color-gradient trail (blue=old → red=current) with a pulsing dot at the head
- Live numeric readout at the bottom (`Raw Data`), showing the same values the
  trail is drawn from, so it follows `Smoothing` and the noise floor
- Soft noise floor: magnitudes below 0.01G shrink to zero
- Z is negated on the way in: iOS reports motion negated against the spec, and
  the screen rotation absorbs that 180 for X/Y but leaves Z upside down
- Motion axes are fixed to the hardware, so X/Y are rotated into the screen
  frame; the buffered trail is rotated too, so history survives a turn
- The angle comes from `window.orientation`, then `screen.orientation.angle`.
  That order matters: iPadOS leaves `screen.orientation.angle` at 0 however the
  device is held, so asking it first reads a confident zero and stops
- Clear Trail, Pause, Keep Awake (screen wake lock), 🔋 Saver, Smoothing,
  Invert Axes and Raw Data controls, two rows
- 🔋 Saver toggles the render tier: off (default) is 60fps / DPR 3 / 480 trail points
  / 20Hz readout, on is 30fps / DPR 2 / 360 points / 10Hz readout

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
