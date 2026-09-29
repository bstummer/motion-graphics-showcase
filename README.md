# Claude — Motion Reel 2026

A 15-second motion graphics showreel in a single self-contained file: [`index.html`](index.html).
Every frame is drawn with Canvas 2D and every sound is synthesized with the Web Audio API.
There are no image or audio files; the only external requests are two Google Fonts
(Instrument Serif and JetBrains Mono).

Open `index.html` in a browser and click the prompt. **Space** pauses and resumes, **R** restarts,
**M** mutes. With `prefers-reduced-motion` set, the page fades in on the end card and offers to
play the reel anyway. Append `?t=7.25` to the URL to render a single still frame at that time.

## The piece

120 BPM, 30 beats. A vermilion text caret is the thread through seven chapters, each one handing
its geometry to the next:

| Beats | Chapter | Handoff |
|---|---|---|
| 0–3 | **Prompt**: `make it move.` is typed on 32nd notes, then the word hops | the caret shoots out as a band and floods the frame |
| 4–7 | **Type**: "Every / frame, / written.", one word per kick; mono code flips into serif | the period winds up and jumps |
| 8–11 | **Form**: one primitive, four corner radii; rotate, morph as an arpeggio, marquee-select | everything is scaled into one dot |
| 12–15 | **Field**: metaballs + topographic iso-lines (marching squares) | the blobs pour back into concentric rings |
| 16–19 | **Space**: the contour map rises into terrain, then a sphere of code glyphs (half-time) | seen edge-on, every ring is a line |
| 20–25 | **Code**: the lines tokenize into a code minimap and accelerate over the riser | it all collapses into the caret |
| 26–29 | **Signature**: the caret fires on the hit and writes the name | hold, caret blinking on the beat |

## How it's built

- **One clock.** `render(t)` is a pure function of the timeline time `t`. While sound is running,
  `t` is derived from `performance.now()` mapped onto the audio output timestamp, so picture and
  sound share a zero point. Pause, restart and resize are exact because nothing accumulates.
- **One cue table.** Scene cuts (`CUE`) are defined in beats, and both the visuals and the score
  read from it, so a cut can't drift off its beat.
- **Re-schedulable score.** `scheduleFrom(t0)` books every event still sounding at `t0`, so
  resume/restart just rebuilds the audio session.
- **Stage.** Locked 16:9 at 1920×1080 logical units, letterboxed on black, rendered at the
  device pixel ratio (capped at 2), with a one-step resolution fallback if frames run long.
