# Changelog

Versions follow [semver](https://semver.org/). See [README.md](README.md#versioning).
Releases before 1.34.0 used two-part versions and are listed as they were shown.

## 1.34.0 · 2026-09-26

### Fixed
- Toggling **Keep Awake** on, off and on again quickly could hold two screen wake locks,
  and one of them was never released. So the screen could stay awake even with Keep
  Awake off. Only one wake lock request can be pending now.
- The README said rendering was capped at 30fps and 360 trail points. It's 60fps and
  480 points by default, and 30fps and 360 points with 🔋 Saver.

### Changed
- Toggle buttons report their on/off state to screen readers (`aria-pressed`).
- The version label uses semver: `v1.34.0 · 2026-09-26`.

### Internal
- `RENDER_POINTS_MAX` is derived from the render tiers instead of repeated.
- Added a meta description and CHANGELOG. Moved the guidance from CLAUDE.md into the
  README and removed CLAUDE.md.

## 1.33 · 2026-09-21
- Negate X and Y too, not just Z.

## 1.32 · 2026-09-18
- Flip the Z axis to match X and Y.

## 1.31 · 2026-09-18
- Drop the leftover double angle resolve.

## 1.30 · 2026-09-18
- Remove the self-update machinery.

## 1.29 · 2026-09-18
- Read `window.orientation` first; delete the scaffolding around it.

## 1.28 · 2026-09-18
- Add a Rotate control; stop inferring orientation from window shape.

## 1.27 · 2026-09-18
- Resolve the screen angle when the APIs don't report one.

## 1.26 · 2026-09-18
- Re-check for updates, not just at load.

## 1.25 · 2026-09-18
- Respect screen orientation.

## 1.24 · 2026-09-18
- The readout follows the Smoothing toggle.

## 1.23 · 2026-09-18
- Self-heal a cached copy of the page.

## 1.22 · 2026-09-18
- Label the readout toggle Raw Data.

## 1.21 · 2026-09-18
- Use a battery glyph on the Saver button.

## 1.20 · 2026-09-18
- Spell out the control labels.

## 1.19 · 2026-09-18
- Put a bolt on the Saver button.

## 1.18 · 2026-09-18
- Add the Saver toggle for a two-tier render budget.

## 1.17 · 2026-09-18
- Rename the app to Ride Report, and write a real README.

## 1.16 · 2026-09-18
- Reserve the safe-area insets.

## 1.15 · 2026-09-18
- Fix canvas label collisions exposed by the one-row controls.

## 1.14 · 2026-09-18
- One-row controls, drop the Floor toggle, area-average the trail.

## 1.13 · 2026-09-18
- Add a soft noise floor at 0.01G behind a Floor toggle.

## 1.12 · 2026-09-18
- Remove the non-linear scale, back to linear.

## 1.11 · 2026-09-18
- Non-linear scale, sensor fallback, performance pass, brighter text.

## 1.10 · 2026-04-03
- Switch units from m/s² to G. Add the date after the version.

## 1.9 · 2026-04-03
- Simplify the Raw toggle to show or hide the readout only.

## 1.8 · 2026-04-03
- Add Smooth, Invert and Raw toggles.

## 1.7 · 2026-04-03
- Smooth the trail: exponential smoothing on history, curve rendering. Make the version
  label more visible.

## 1.6 · 2026-04-03
- Invert the axes to show the felt direction, not the acceleration direction.

## 1.5 · 2026-04-03
- Add the Z-axis bar.

## 1.4 · 2026-04-02
- Redesign for aircraft turbulence tracking.

## 1.3 · 2026-04-02
- Add a subtle version label.

## Earlier · 2026-04-02
- First version: a device motion tracker, then a fix for the trail not rendering.
