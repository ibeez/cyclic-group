---
type: design
---
# Fig. 13 · The Cyclic Group (design)

For the builder. Tactile ASCII exploration of the cyclic group Z / 6Z (clock addition).

## Intent
Fig. 13 in Judy's "math facts that live in my head" series.
The math fact: Z / 6Z, actions you can always undo. Six perches [0] through [5] on a clock dial.
The moment: a hop around the clock either visits every perch (full generator), locks into an equilateral triangle (subgroup of order 3), or bounces across the diameter (subgroup of order 2). Every step has an inverse that brings you back home to [0].

## The Math Truth
- Group: (Z / 6Z, +), the integers modulo 6 under addition.
- Perches: [0], [1], [2], [3], [4], [5].
- Identity: +0 keeps you in place. Adding 6 steps brings you full circle back to [0].
- Generators (coprime to 6):
  - +1 visits all six perches in clockwise order: 0 -> 1 -> 2 -> 3 -> 4 -> 5 -> 0.
  - +5 visits all six perches in reverse (counter-clockwise): 0 -> 5 -> 4 -> 3 -> 2 -> 1 -> 0.
- Subgroups (divisors of 6):
  - +2 generates the order 3 subgroup <2> = {0, 2, 4}. This draws an equilateral triangle. Odd perches {1, 3, 5} are unreachable.
  - +3 generates the order 2 subgroup <3> = {0, 3}. This bounces across the diameter like a pendulum.
  - +4 generates the same order 3 subgroup in reverse: 0 -> 4 -> 2 -> 0.
- Inverses (actions that cancel):
  - +1 and +5 cancel out: 1 + 5 = 6 = 0 (mod 6).
  - +2 and +4 cancel out: 2 + 4 = 6 = 0 (mod 6).
  - +3 is its own inverse: 3 + 3 = 6 = 0 (mod 6).

## ASCII pass (11/3, after ASCII-LESSONS.md)
- No ruled lines through the figure: the faint chord web is gone. A hop across the dial leaves a trail of bold warm `hop` letters on the tight grid; neighbour hops turn that ring sector bold, warm and larger (bold part, faint rest).
- Ring is tight repeating `hop` (12.5 px step), one faint indigo. No blue flicker, no hop/turn mix (meaningless variance).
- Perches are plain Gaegu `[k]`; current one is warm and larger. Pink halo removed.
- Buttons are a plain word row (`+1` over `all six`), no boxes: one frame per plate.
- Traveller (11/3): no dots. The path's own letters swell big (up to 33 px), bold and orange (tapering to Sand) as the hop passes, then settle into the coral trail. Letters ahead of the head stay faint.
- Buttons, keys 1 to 5 and Space run the full cycle from [0] back to [0] (+1/+5 six hops, +2/+4 three, +3 two), trace cleared first. Arrows and perch clicks stay single hops.
- Old version: `Trashcan/cyclic-group-index-2026-11-03-before-ascii-pass.html`. preview.png and the mp4 are not re-exported yet.

## What It Is
- Visual style: ASCII on warm paper (`#fbf5e6` seamless tile). Single thin brass boundary rule.
- Palette: Judy's markers from `palette.js`. Warm Coral 208 and Papaya 217 for active visits; POSCA Indigo and Dark Blue Light 15 for cool inactive perches and ring letters; Blue Grey B47 for text, frame, and ink rules. Paper is the only literal hex.
- Plate size: 1200 x 1000, centered layout.
- Dial ring: tight lowercase words along the curve ("hop hop hop" or "turn turn turn"), upright letters, lower half reading left to right.
- Six perches: bracketed ASCII tokens [0] through [5], spaced at 60 degree increments with [0] at 12 o'clock. Active nodes light up bold and warm; inactive nodes stay faint cool indigo.
- Chord lines: faint cool chords between perches when resting; active visited chords light up bold warm coral and papaya, tracing out the hexagon, the triangle, or the diameter.
- The traveller: tapered big dots (POSCA Orange into Sand) that hop from perch to perch along the arc or chord.
- Header: Gaegu 700 at 66 px: `Z / 6Z · clock addition` with gentle staggered heartbeat.
- Status line: centered at y = 158, Gaegu 700 at 34 px, 1 to 4 words (e.g. `visits all six`, `triangle subgroup`, `across the diameter`, `backwards generator`, `home to zero`).
- Hop formula: below status line, a clean small display showing `[a] + [b] = [c]`.

## The 16 s Loop (Autoplay Reel)
480 frames at 30 fps, 120 BPM (8 bars of 4/4, 2.0 s per bar):
- 0.0 s to 4.0 s (Bars 1 and 2): +1 Generator. Hops 0 -> 1 -> 2 -> 3 -> 4 -> 5 -> 0. Hexagon edges light up one by one. Caption: `visits all six`.
- 4.0 s to 8.0 s (Bars 3 and 4): +2 Triangle Subgroup. Hops 0 -> 2 -> 4 -> 0. Equilateral triangle chords light up. Caption: `triangle subgroup`.
- 8.0 s to 12.0 s (Bars 5 and 6): +3 Diameter Subgroup. Hops 0 -> 3 -> 0. Diameter chord lights up. Caption: `across the diameter`.
- 12.0 s to 16.0 s (Bars 7 and 8): +5 Reverse Generator. Hops 0 -> 5 -> 4 -> 3 -> 2 -> 1 -> 0. Reverse hexagon edges light up. Caption: `backwards generator`.
- At 16.0 s: lands on [0], trace resets, and loop joins frame 0 invisibly.

## State Machine & Interactive Toy
- State:
  - `perch`: current perch index (0 to 5).
  - `targetPerch`: destination perch during hop.
  - `hopProgress`: 0 to 1 during hop animation.
  - `activeChords`: set of currently illuminated chords.
  - `visitedPerches`: set of perches lit in the active action.
  - `demo`: boolean flag (true during autoplay loop, false upon user action).
  - `idleTimer`: countdown that returns to demo reel after 10 s of inactivity.
- Controls:
  - On-canvas tactile hop buttons under dial: [+1], [+2], [+3], [+4], [+5], and [0 / reset].
  - Direct perch click: clicking any perch hops directly to it with the corresponding relative shift.
  - Keyboard: keys 1 to 5 hop +1 to +5. Space repeats last hop. 0 or R resets to [0]. Arrow keys hop +1 (right/up) or +5 (left/down).
  - Hidden accessible buttons in HTML controls for screen readers.

## Audio Design
Synthesized in Web Audio, original sounds:
- Key: C major hexachord / pentatonic.
- Musical pitch per perch:
  - Perch [0]: C4 (261.6 Hz, root)
  - Perch [1]: D4 (293.7 Hz)
  - Perch [2]: E4 (329.6 Hz, major third)
  - Perch [3]: G4 (392.0 Hz, perfect fifth)
  - Perch [4]: A4 (440.0 Hz, major sixth)
  - Perch [5]: C5 (523.3 Hz, octave)
- Musical rationale:
  - +1 plays the ascending pentatonic melody.
  - +2 arpeggiates the bright C-E-A triad.
  - +3 rings the pure clock chime fifth C4 to G4.
  - +5 plays the descending retrograde melody.
- Background rhythm: soft music box / celesta lead and triangle bass at 120 BPM, with a DynamicsCompressor and ducking under sound effects.
- Sound effects:
  - Perch landing: soft wooden tap and ringing chime on that perch's note.
  - Loop closure: soft held chime chord when completing a cycle at [0].
  - Button click: crisp wooden tick.
