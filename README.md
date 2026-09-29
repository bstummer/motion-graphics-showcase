# Claude — Motion Reel 2026

A 15-second motion graphics showreel in a single self-contained file: [`index.html`](index.html).
Every frame is drawn with Canvas 2D and every sound is synthesized with the Web Audio API.
There are no image or audio files; the only external requests are two Google Fonts
(Instrument Serif and JetBrains Mono).

Open `index.html` in a browser and click the prompt. The previous cut is kept unchanged as
[`reel-v1.html`](reel-v1.html); [`CRITIQUE.md`](CRITIQUE.md) is the review that drove v2.

### Controls

| Key / gesture | Action |
|---|---|
| click / tap the prompt | play (sound on) |
| **Space**, or click / tap the picture | pause · resume |
| **R** | restart |
| **M** | mute |
| **← / →** | step to the previous / next beat (every scene cut is on a beat) |
| **⇧← / ⇧→** | step one frame |
| drag the ruler at the bottom | scrub (mouse or touch); it appears on pause or when the pointer moves |
| **L** | seamless loop: the caret takes the name back and returns to the prompt on beat 30 |
| **O** | director's overlay: safe areas, cue ruler with every motion-blur window, frame / beat / cost readout |
| **B** | true motion blur on / off |
| **P** | post-processing on / off |

With `prefers-reduced-motion` set, the page fades in on a still end card (no blinking, no grain
motion) and offers to play the reel anyway. URL flags: `?t=7.25` renders a single still,
`?loop=1`, `?overlay=1`, `?blur=0`, `?post=0`.

## The piece

120 BPM, 30 beats. A vermilion text caret is the thread through seven chapters, each one handing
its geometry to the next:

| Beats | Chapter | Handoff |
|---|---|---|
| 0–3 | **Prompt**: `make it move.` is typed on 32nd notes, then the word hops | the caret shoots out as a band and floods the frame |
| 4–7 | **Type**: "Every / frame, / written.", one word per kick; mono code flips into serif | the period winds up and jumps |
| 8–11 | **Form**: one primitive, four corner radii; rotate, morph as an arpeggio, marquee-select | everything is scaled into one dot |
| 12–15 | **Field**: metaballs + topographic iso-lines (marching squares) | the blobs pour back into concentric rings |
| 16–19 | **Space**: the contour map rises into terrain, then a sphere set in its own source code, with depth of field (half-time) | seen edge-on, every ring is a line |
| 20–25 | **Code**: a lens runs through the lines and code condenses in its wake; the plane tilts away as it accelerates over the riser | the plane swings edge-on into the playhead, which folds into the caret |
| 26–29 | **Signature**: a two-frame impact frame, then the caret fires and the letters stamp down | hold, caret blinking on the beat (or, looping, back to the prompt) |

## How it's built

- **True motion blur.** A 180° shutter: fast moments are the average of 3–16 sub-frames spread
  across 1/120 s, which works because `render(t)` is pure. Sample counts are set per window (the
  overlay shows them). The shutter never opens across a cut, the impact frame stays crisp, and a
  slow machine sheds samples before it sheds resolution.
- **Post-processing.** One WebGL pass over the finished frame: red-led halation (the warm bleed
  off bright, saturated edges), a lens kick and faint chromatic split on the cuts, luminance-
  weighted grain and a vignette, graded on a continuous scene-lightness curve so nothing jumps.
  The HUD sits above it, crisp. Without WebGL the reel falls back to the CSS grain.
- **The sphere reads itself.** The glyphs on the SPACE rings are `drawSpace()`, read at runtime
  out of the page's own `<script>`: keywords in vermilion, comments dimmed.
- **Seamless loop.** Loop mode books the next pass of the score two seconds early on the same
  audio clock, so the hit's tail rings into the next prompt; the picture lands on the exact
  first frame.

- **One clock.** `render(t)` is a pure function of the timeline time `t`. While sound is running,
  `t` is derived from `performance.now()` mapped onto the audio output timestamp, so picture and
  sound share a zero point. Pause, restart and resize are exact because nothing accumulates.
- **One cue table.** Scene cuts (`CUE`) are defined in beats, and both the visuals and the score
  read from it, so a cut can't drift off its beat.
- **Re-schedulable score.** `startAt(t0)` opens a fresh audio session and `score()` books every
  event still sounding at `t0`, so resume and restart just rebuild the session.
- **Loading.** Nothing blocks the first paint: the font stylesheet loads asynchronously (DOM text
  uses `font-display: swap`; the canvas waits for the real faces itself). Boot runs in two stages.
  The play screen is drawn as soon as JetBrains Mono arrives; the serif layouts, glyph atlas and
  WebGL pass are then built one slice per frame, with shaders compiled in parallel where the
  browser supports it. The glyph atlas takes six whole-bank passes rather than a blur filter per
  glyph, and the blur buffer and overlay canvases are allocated only when they're needed. At rest,
  the grain re-runs one shader pass on the frame already on the GPU instead of re-rendering it.
- **Stage.** Locked 16:9 at 1920×1080 logical units, letterboxed on black, rendered at the
  device pixel ratio (capped at 2). DOM controls keep a minimum size, so they stay usable on phones.
