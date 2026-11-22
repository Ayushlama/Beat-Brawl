# Beat Brawl

Two stickmen fight in time with whatever music is playing. One HTML file, no build step, no dependencies.

## What the music does

| Sound | Move |
| --- | --- |
| Bass (kick drum, low synth) | Jab, cross, **Flash Step** dash with afterimages, **Rising Fist** uppercut that launches the opponent into a flip |
| Snare | Kick, **Meteor Kick** flying kick. With a full energy bar the next snare fires a **Blast Wave** that cannot be dodged |
| Hi-hat | Defender tries to dodge. A dodge that makes the attack whiff swaps who is attacking |

Landing hits fills the energy bar. Big moves add speed lines, a camera push-in, hit-stop and screen flash.

## Song-length pacing

When you load a song the page reads its duration and a loser is picked at random. Each fighter has a target health curve over the length of the song:

- the loser goes from 100% down to 0% at the final second
- the winner ends around 40%

Hits snap health to that curve, with a floor so nobody can be knocked out early. The loser goes down when the music ends. The demo beat loops forever, so it plays in rounds instead.

## How it works

1. **Audio**: the Web Audio API plays the song (or a built-in synthesized beat) through an `AnalyserNode`.
2. **Onset detection**: each frame the spectrum is split into bass (30-150 Hz), snare (1.5-5 kHz) and hi-hat (7-14 kHz). Each band is measured in linear amplitude and fires when its energy jumps above a noise floor, its own recent average and a fraction of its own recent peak. The `bands` object and `analyse()` function hold this. The sensitivity slider scales the thresholds.
3. **Fight logic**: each onset triggers a move for the current attacker.
4. **Animation**: a stickman is a set of joint angles. Moves are keyframed poses interpolated over time (`ACTS`), and the hip height is derived from the lowest foot.
5. **Rendering**: everything is drawn on a `<canvas>`.

Fonts (Bungee, IBM Plex Mono) load from Google Fonts and fall back to system fonts offline.

