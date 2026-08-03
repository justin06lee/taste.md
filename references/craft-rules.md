# Craft rules

How to make the drawing itself good, once the idea is settled. These are the
decisions that separate "clearly deliberate" from "clearly generated."

---

## Restraint, and the SVG detail ceiling

Hand-authored SVG is not painting. Every curve is a coordinate you reasoned
about, so detail costs enormously more here than in a rendered illustration —
and it *looks* like it cost more, which is the problem. Fussy hand-placed detail
reads as stiff; the same subject reduced to six confident shapes reads as
designed.

So the medium has a ceiling, and the right response is to design under it rather
than fight it:

| Cheap in SVG, and reads well | Expensive in SVG, and reads badly |
|---|---|
| One object with a strong outline | A character in a pose holding a thing |
| Flat fills, one gradient each | Stacked soft shading |
| Two or three big forms | Many small overlapping parts |
| A bold repeated motif | Hand-placed texture, fur, individual hairs |
| A silhouette with one cut-out | Fingers, whiskers, teeth, scattered confetti |

**The reduction test.** Draw it, then delete one element. Still recognizable?
Delete another. Keep going until removing something breaks it, then put that
last one back. Whatever survives is the mark. Almost every first draft has two
or three elements that are doing nothing but proving effort.

**When the subject genuinely needs richness** — a full character, an
environment, painterly depth — that is not a reason to grind harder in SVG. It
is a signal to say so and ask whether a real illustration is wanted instead.
Producing a stiff vector approximation of a rendered image serves nobody, and
the header-image question in Part 2 exists precisely so this comes up before
the work rather than after.

## Form

**Fewer, bigger shapes.** Five distinct elements is already a lot. Every element
you add subtracts legibility at small sizes, and small sizes are where the icon
actually lives.

**One focal element, 55–70% of the safe area.** Smaller and it looks timid and
lost in its own frame; larger and it feels cramped against the edges. Everything
else in the composition is support.

**Optical centering, not mathematical centering.** Visual weight is not
geometric area. A mark that is heavy at the bottom needs to sit slightly *high*
to look centered — usually 1–3% of the canvas. Circles look small next to
squares of the same bounding box, so they should overshoot slightly.

**Overshoot on curves.** A circle whose top exactly meets a flat edge reads as
slightly short. Round shapes need to extend a touch past a flat neighbour's
boundary to look aligned.

**Stroke weight scales inversely with target size.** A stroke that looks elegant
at 1024 is invisible at 32. Author small-size variants with strokes that look
almost comically fat on the big canvas — they resolve correctly at the size
they're for.

**Rotate something.** A composition where every element sits on the horizontal
or vertical axis reads as static and clip-art-ish. A single element rotated
8–15° off-axis introduces life without introducing chaos.

## Colour

**Two hues maximum, plus neutrals.** Three is possible with discipline. Rainbow
gradients, and the multi-stop hue sweeps that design tools default to, read as
cheap and always have.

**Short hue travel in gradients.** Two or three stops moving through a narrow
range — a deep shade to a bright one *of the same family* — reads as light
falling on an object. A gradient traveling from purple to orange reads as a
gradient, which is not the goal.

**Never pure `#000` or pure `#fff` in large fills.** Tinted near-blacks and
near-whites (a near-black carrying a little of the dominant hue, a near-white
carrying a little warmth) look like objects. Pure values look like defaults.

The exception is absolute: **macOS template images must be pure `#000` + alpha.**
That is not a colour decision, it is a format requirement.

**Check it in grayscale.** If the focal element and the background collapse to
the same value, the icon is carried entirely by hue. It will disappear for
colour-blind viewers, on low-quality displays, and in any monochrome rendering
context. Fix by separating *value*, not by changing hue.

**Test on a busy background.** Icons live on wallpapers, not on white. An icon
that only works on a neutral field is not finished.

## Light and dimension

**One light source, consistently.** Pick a direction — upper left is the
convention and it's the convention for good reasons — and let every element in
the icon obey it. Mixed lighting is the single most reliable tell of an amateur
icon, and it is entirely avoidable.

**Self-shade with an off-center radial gradient.** Rather than painting a
highlight shape on top of a flat fill, fill the object with a radial gradient
whose center sits toward the light:

```svg
<radialGradient id="body" cx="0.34" cy="0.28" r="0.88">
  <stop offset="0"    stop-color="#fffdfc" />
  <stop offset="0.55" stop-color="#ffeee9" />
  <stop offset="1"    stop-color="#f3c0b4" />
</radialGradient>
```

The object now shades itself. This survives downscaling; a painted-on highlight
becomes a smudge.

**No drop shadows, no bevel-and-emboss.** Both turn to mud below 64px, and both
read as dated at every size. Depth comes from the gradient and from shape
overlap.

**Ambient glow, used sparingly.** A very low-opacity radial over the background
(`opacity: 0.13`-ish) can suggest light without becoming a visible element. If
you can see it *as* a glow, it's too strong.

## Legibility

**The silhouette test.** Fill the whole thing solid black and view at 32px.
Still recognizable? If not, no amount of colour work will save it. Fix the
shape. This is the highest-value check in the entire process and it takes
seconds.

**The squint test.** Squint at it. What survives is what people actually
perceive at a glance. If the thing that survives isn't the point of the icon,
the hierarchy is wrong.

**The 16px test.** Render it at 16 and look at it honestly. This is where an
icon spends most of its life — browser tabs, file lists, tray. If it fails,
that's what the optical-size variant is for; don't pretend the big one is fine.

**Punch details out with masks, don't paint them.** A cut-out shape stays
transparent, works on any background, and tints correctly under a system tint. A
shape painted in the background's colour breaks the instant either changes:

```svg
<mask id="grin">
  <rect width="72" height="72" fill="#fff" />
  <path d="…" fill="#000" />   <!-- black = punched out -->
</mask>
<g mask="url(#grin)"><circle cx="33" cy="45" r="22" /></g>
```

**No text.** Letterforms are illegible below ~64px, and an icon that needs its
name written on it hasn't done its job. A single letterform *as* the mark can
work — but only as a genuine structural pun, never as a fallback.

## Small-size adaptation

The changes that make a small variant work, roughly in order of impact:

1. **Delete** incidental detail — sparks, texture, secondary elements, fine
   linework. Aim for three shapes.
2. **Thicken** every stroke relative to the canvas.
3. **Raise contrast** between the focal element and the background.
4. **Enlarge** the focal element, trimming margin.
5. **Simplify curves** — fewer control points, larger radii.

The small variant is a *different drawing of the same idea*, not a compressed
copy of the large one. That is the professional standard, and it is why
professionally made icon sets stay crisp everywhere.

## Documenting the drawing

Put a header comment in the SVG covering:

- **What the mark means** — the double-meaning sentence from step 2
- **Why the canvas is what it is** — the platform grid, the safe zone
- **Why any surprising number is what it is** — stroke widths chosen for a
  downscale target, an opacity tuned by eye, an off-axis rotation

Reze's tray glyph carries this, and it is the reason the file is safe to touch
a year later:

```
  A macOS *template* image: pure black plus alpha, nothing else. The system
  ignores the colour and tints the alpha channel itself, so this comes out
  white on a dark menu bar and black on a light one automatically. Drawing it
  white here would break that.
```

Without that comment, the next person "fixes" the black glyph by making it
white, and breaks it in light mode.
