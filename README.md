# Ride Report

A single-page ride-quality meter for your phone. Lay it flat on a tray table and it
draws what the aircraft is doing to you, live.

**[jkastl.github.io/ride-report](https://jkastl.github.io/ride-report/)**

No build step, no dependencies, no network calls, no data leaves the device.
The whole app is one file: [`index.html`](index.html).

## What you're looking at

The plot is **acceleration, not position**. The center of the canvas is 0G — perfectly
smooth air. The dot's distance from center is how hard you're being pushed right now,
and the trail is the last 15 seconds of that, fading blue (old) to red (current).

- **Left/right** on screen is lateral acceleration — sway, dutch roll, a gust on the tail
- **Up/down** on screen is fore/aft — thrust changes, drag, the shove when spoilers deploy
- **The bar on the right edge** is vertical (Z), the one that actually makes drinks jump
- **Rings** mark 0.12G, 0.3G and 0.5G; 0.5G fills the canvas
- **The word at the top** is the running peak, decaying over a few seconds

A calm cruise looks like a small scribble near the middle. Chop stretches it into a
fuzzy star. A single hard jolt throws a red streak toward one edge.

## Controls

| Button | What it does |
| --- | --- |
| **Clear Trail** | Wipe the trail and reset the peak |
| **Pause** | Stop sampling; the trail freezes and the render loop stops |
| **Smoothing** | Exponential smoothing, ~0.2s time constant. Off shows the raw signal |
| **Invert Axes** | Plot the direction you're thrown rather than the direction the aircraft accelerates |
| **Keep Awake** | Hold a screen wake lock so the phone doesn't sleep mid-flight. Off by default |
| **🔋 Saver** | Halve the render cost for long unplugged rides. Off by default |
| **Readout** | Show the live numeric X/Y/Z readout along the bottom. Unrelated to `Smoothing` — it changes what is displayed, not what is measured |

### Render tiers

`🔋 Saver` picks how hard the phone works to draw the trail. Everything else —
sampling, smoothing, the peak and the turbulence label — is identical in both.

| | Saver off (default) | Saver on |
| --- | --- | --- |
| Frame rate | 60fps | 30fps |
| Device pixel ratio | up to 3 | up to 2 |
| Trail points | 480 in 24 stroke batches | 360 in 18 |
| Readout refresh | 20Hz | 10Hz |

## Turbulence levels

| Label | Horizontal peak |
| --- | --- |
| Calm | under 0.03G |
| Light | 0.03G |
| Moderate | 0.12G |
| Severe | 0.30G |

These are this app's own thresholds, picked for what's noticeable at a tray table. They
are **not** the FAA/ICAO turbulence categories, which are defined differently and sit at
considerably higher values. Don't file a report with them.

## Requirements

- **iOS 13+ Safari** — motion access requires a user gesture, hence the *Enable Motion*
  button. It also requires HTTPS, so open the hosted link rather than a local file.
- **Android Chrome** — starts without a prompt. Devices that report only
  `accelerationIncludingGravity` fall back to a gravity high-pass, and the status reads
  `Live · est. gravity` when that path is active.
- A desktop browser will load the page and render nothing, because there's no
  accelerometer to read. The status line will tell you so.

## Signal handling

Worth knowing if the numbers look off:

- Samples are timestamped and everything downstream is time-based, so a 60Hz iPhone and a
  200Hz Android behave identically. Sample counts are not used as a proxy for time.
- A soft noise floor shrinks magnitudes toward zero by 0.01G. This kills sensor jitter at
  rest without making the dot snap as real bumps cross the threshold.
- The trail is drawn at up to 360 points regardless of sample rate. Each point is the
  **mean** of its slice of the buffer, not a sampled representative, so nothing aliases.
- Rendering is capped at 30fps and stops entirely while paused or backgrounded.

## Development

Edit `index.html`. That's the whole app; there is nothing to install or build.

```sh
open index.html      # desktop preview, no motion data
```

Push to `main` to deploy — GitHub Pages serves the repo root.

Real testing requires a real device. Simulators don't fire motion events, and a phone
sitting on a desk only tells you what the noise floor looks like.

## Known limitations

- **No barometer.** Cabin pressure is the best available proxy for climb and descent, and
  browsers don't expose it on any platform. It's native-only.
- **The phone's axes, not the aircraft's.** Everything assumes the phone lies flat and
  stays put. Pick it up and you're measuring your hand.
- **Acceleration only.** No attitude, no heading, no altitude. Yaw in particular is not
  worth showing: it drifts on Android and relies on a magnetometer inside an aluminum
  tube on iOS.
- **Thresholds are unvalidated.** They have never been checked against a real
  instrumented measurement.
