# gurdjieff meditations

**Still Moving.** Four movements that stop the mind. A nine minute activity sheet inspired by the sacred dances of G. I. Gurdjieff.

One HTML file. No dependencies, no build step, no framework, no tracking.

---

## The premise

You cannot relax your way out of a busy mind. Trying to calm down is one more item on the list.

Attention has a fixed size. Give it two jobs it cannot run on autopilot at the same time and there is nothing left over to worry with. Thought is not silenced. It runs out of room.

That is the mechanism behind Gurdjieff's Movements: limbs set to different rhythms at once, often three against four, with counting and body sensing layered on top. Habit and symmetry stripped out on purpose. This site takes that mechanism and scales it down to something a stressed person can do in nine minutes with no training.

---

## What is in it

| # | Movement | Time | Where |
|---|----------|------|-------|
| 01 | Two Places At Once | 90 sec | Seated, anywhere |
| 02 | Three And Four | 3 min | Standing, out loud |
| 03 | The Stop | 2 min | Standing, needs a timer |
| 04 | The Standing Sit | 60 sec | Either |

Each movement page follows the same shape: Setup, Do, What Happens, If It Goes Wrong, When To Use It.

---

## Interactive parts

### Polyrhythm player (Movement 02)

A Web Audio metronome that plays the three against four pattern so you can hear it before you can do it.

- Low tone on beats 1, 5, 9 (feet, counting in four)
- High tone on beats 1, 4, 7, 10 (arms, counting in three)
- Faint tick on the empty beats so you can keep the count
- Both accents land together on beat 1, every twelve

Modes: **Feet**, **Arms**, **Both**. These match steps 1, 2 and 3 of the written instruction. You can switch mode mid playback.

The SVG grid animates off the same clock. A playhead sweeps the twelve beats, dots fire as the tones hit, and a live readout shows `Feet 3 · Arms 2` so both counts are visible at the same time.

Timing runs on the Web Audio clock with a 120 ms lookahead scheduler, not `setInterval`, so it does not drift.

### Guided runner

Full screen, seventeen steps, about 8 minutes 31 seconds. One instruction at a time, auto advancing, with a progress bar and a countdown.

- During the "now both" step it drives the polyrhythm engine directly
- Pause, Skip, Exit
- Keyboard: `Esc` exits, `Space` pauses, `→` skips

Launch from the cover button or the **Run** link in the nav.

### The Stop

The exercise requires that the signal come from outside you. If you pick the moment, you pick a comfortable posture, and nothing happens. So the page provides the signal.

1. Screen goes black. "Move. Walk, sway, reach. No pattern."
2. At a random point inside thirty seconds it slams to full screen gold with a low hit and a vibration
3. Counts the twenty second hold
4. Release

Runs standalone from Movement 03, or inline inside the full sequence.

### The interface stops

Scroll into The Standing Sit and the nav fades out. Nothing is left but the circle. The design does what the page says.

---

## Run it locally

```bash
git clone https://github.com/YOUR_USER/gurdjieffmeditations.git
cd gurdjieffmeditations
```

Open `index.html` in a browser. That is the whole workflow.

If you want a local server (not required, but avoids any `file://` quirks):

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

---

## Deploy

### Vercel

1. Push `index.html` to the repo root
2. Go to [vercel.com/new](https://vercel.com/new) and import the repo
3. Framework Preset: **Other**. Leave Build Command and Output Directory empty
4. Deploy

No `vercel.json` needed. Every push to `main` redeploys.

### Anywhere else

Drag `index.html` onto Netlify Drop, or push to GitHub Pages, or upload it to any static host. It is one file.

---

## Project structure

```
gurdjieffmeditations/
├── index.html    # everything: markup, styles, scripts, SVG
└── README.md
```

Inside `index.html`:

| Region | What it holds |
|--------|---------------|
| `<style>` | CSS custom properties, layout, responsive rules, print sheet |
| `<nav>` | Sticky bar, section links, Run trigger |
| `<header class="cover">` | Full viewport cover |
| `#premise` | Problem, Method, Why, Three Rules, Sequence index |
| `#m1` to `#m4` | The four movements |
| `#run` | Runner overlay markup |
| `<script>` | Audio helpers, rhythm engine, player widget, runner, observer |

---

## Customising

### Colours

All colour lives in `:root` at the top of the stylesheet.

```css
:root{
  --paper:#E8DFCF;   /* warm bone background */
  --ink:#171310;     /* body text */
  --brass:#9A6E22;   /* primary accent */
  --rust:#7A4C2A;    /* feet accents in the rhythm grid */
  --deep:#16181C;    /* dark section background */
  --gold:#C9A25A;    /* dark section accent, the Stop flash */
}
```

### Tempo

```js
var SPB=1.0;  // seconds per beat
```

Lower is faster. `1.0` gives a twelve second cycle, which is slow enough to count out loud.

### The guided sequence

The runner reads a plain array. Edit `FULL` to change wording, order or timing.

```js
{m:'Two · Three And Four', t:85, s:'Now both. Feet in four. Arms in three.', click:'both'}
```

| Key | Meaning |
|-----|---------|
| `m` | Movement label shown top left |
| `t` | Duration in seconds |
| `s` | The instruction |
| `click` | Runs the metronome: `'feet'`, `'arms'` or `'both'` |
| `bg` | Background for this step: `'dark'` or `'gold'` |
| `rand` | Ignore `t`, use a random 10 to 30 seconds |
| `bang` | Fire the loud signal and vibration instead of the soft chime |

`STOPONLY` is the four step version used by the button on Movement 03.

### Fonts

Jost for display, Spectral for body, loaded from Google Fonts. Fallbacks are Futura / Century Gothic and Palatino / Georgia, so the site still looks right offline. To go fully self contained, drop the two `<link>` tags and the stack falls back on its own.

---

## Browser support

Modern evergreen browsers: Chrome, Edge, Firefox, Safari, plus iOS and Android.

Uses CSS Grid, `clamp()` with pixel fallbacks, custom properties, `position: sticky`, `IntersectionObserver`, and the Web Audio API. Audio requires a user gesture to start, which is why nothing plays until you press Play or Run. `navigator.vibrate` is used when present and skipped silently when not.

Responsive breakpoints: 520, 600, 620, 640, 760 and 900 px.

---

## Accessibility and print

- Semantic headings and landmarks, real buttons, `aria-pressed` on the mode selector, `aria-live` on the runner instruction
- Every SVG carries `role="img"` and a label
- `prefers-reduced-motion` disables smooth scrolling
- Full keyboard control inside the runner
- A print stylesheet hides the nav, player and overlay and breaks each movement onto its own page, so `Cmd/Ctrl + P` still produces the five page task sheet

---

## Sources

1. Limbs are set to independent contrapuntal rhythms, often triple against quadruple, with counting and sensing layered on top. James Moore, *Gurdjieff's Dances and Movements*, gurdjieff.org.uk
2. The Stop requires an external command and is held without adjustment. P. D. Ouspensky, *In Search of the Miraculous*. Also R. Oksanen, *The Stop Exercise*.
3. Sensing is foundational in the Fourth Way and commonly accompanies the Movements. *Sensing Exercises*, endlesssearch.co.uk
4. The six Obligatories are the preliminary exercises in attention and coordination, taught before all else. *Gurdjieff's Movements and Sacred Dances*, gurdjieffandfourthway.org

---

## A note on lineage

The Movements are transmitted in person by teachers in the Gurdjieff lineage, with hands on correction. This project is inspired by that work. It is not that work, and it is not a substitute for it. If the material interests you, find a teacher.

Formal research on the Movements is limited. Nothing here is medical advice. If you have balance problems, joint injuries, or a condition affected by exertion, adapt or skip the standing movements.

---

## Licence

Code: MIT. Do what you like with it.

Text and design: yours to adapt, but please do not present it as authentic Gurdjieff Movements instruction.
