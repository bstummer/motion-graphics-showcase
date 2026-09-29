# Creative director's critique of v1

Written before any v2 changes, from the v1 contact sheets (8 frames/s) and full-res stills.
v1 is preserved unchanged as [`reel-v1.html`](reel-v1.html).

## The three weakest moments

### 1. 10.00–11.00 s (beats 20–21): the ruled lines
Twenty-nine identical 2 px lines, evenly spaced from edge to edge, and a bump on beat 21 you
have to look for. It reads as notebook paper or a screensaver. There is no hierarchy and no
focal point, and the beat doesn't land visually. It's the least designed second in the reel,
and it comes right after the best one. The lines also turn into code on beat 22 for no visible
reason: nothing *causes* the tokenizing.

### 2. 12.10–12.60 s (beats 24–25): the riser peak
The speed is faked by stretching every token into a fat, grey, rounded capsule. It reads as a
low-res blur filter, not as velocity. The zoom pushes the code block into the left 55% of the
frame: the right side is empty, and the gutter numbers slide under the HUD slate at top left.
The collapse into the playhead is a plain vertical squash, even though the previous scene
established a better rule (a plane seen edge-on becomes a line).

### 3. 13.00–13.15 s (beat 26): the hit
It's the loudest moment of the score and the thinnest moment of the picture. The hit frame is a
flat tinted frame with a caret in it. The reveal is a hard clip wipe dragging a muddy brown
gradient box across the letters. The name arrives at its final size with no weight or settle.
The climax needs conviction: an impact frame, a real motion streak and letters that land.

## The signature shot: 8.40–9.60 s (beats 17–19), the sphere of code
The contour map rising into terrain and closing into a sphere around the vermilion core is the
one image in the reel only this designer would make. Right now the glyphs are random symbols,
uniformly sharp, the sphere is modest in frame and the camera barely moves. To earn the
signature slot:
- the rings should be written in **the reel's own source code**, readable in sequence, so
  the image is literally made of the code that draws it
- real **depth of field**: the far side soft and dim, the near side crisp
- a slow **dolly-in** that hands back exactly to the edge-on lines
- **halation** on the core and true **motion blur** on the yaw whip of beat 18

## Priorities for v2
1. Fix the three moments above and build the signature shot.
2. Then global craft: true temporal motion blur and a post-processing pass (halation, lens,
   grain), used with restraint.
3. Only then new controls (scrubber, overlay, seamless loop).

## Outcome (v2)

1. **Ruled lines → lens wave.** On beat 21 a lens sweeps left to right through the lines, and the
   code condenses out of each line in its wake, so the tokenizing has a cause and a direction.
2. **Riser → a plane tilting away.** The code sits on a plane that tilts back as it accelerates,
   with true motion blur instead of fat capsules. On beat 25 the plane swings edge-on and becomes
   the playhead: the same rule SPACE established.
3. **The hit → impact frame and stamp.** Two frames of inverted paper on the hit, a caret sweep
   with real motion blur, letters that land from 112% and settle, halation and a lens kick.
4. **Signature shot.** The sphere is set in `drawSpace()`, its own source, with depth of field,
   halation on the core, a dolly-in and a motion-blurred yaw whip on beat 18.
