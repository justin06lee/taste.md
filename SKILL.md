---
name: taste
description: Use when designing or generating any icon — app icon, favicon, menu-bar/tray/taskbar glyph, PWA or maskable icon, .icns/.ico/.iconset bundle, or a full multi-size icon set. Triggers on "make an icon", "app icon", "logo for my app", "favicon", "tray icon", "menu bar icon", "taskbar icon", "generate all the icon sizes", "my icon looks blurry/generic/bad at small sizes", and on adding an icon to a Tauri, Electron, PWA, iOS, Android, or web project. Covers inventing an original mark and rendering every platform size from one committed SVG source.
---

# taste

Invent an original mark that means two things at once, then render every platform size from a single committed SVG.

Bad icons fail for three reasons, in this order of frequency:

1. The idea is stock — a gradient blob with a lightning bolt through it.
2. The artwork was authored at the wrong size and downscaled into mud.
3. One file was reused for jobs that need genuinely different files.

Every step below exists to prevent one of those. Work them in order; step 2 is the one that produces the reaction *"how did you even do that"*, and it is the one most likely to get skipped.

## Non-negotiables

1. **Invent, never source.** Do not download, trace, embed, or reproduce existing artwork — characters, logos, stock marks, another app's icon. It is someone else's copyright sitting in the user's bundle, and it caps the work at *retrieval* when the ceiling on *invention* is far higher. When the user offers a reference ("make it look like X"), treat the reference as a source of **meaning**, not a source of pixels.
2. **The mark carries two meanings.** See step 2. This is the whole skill.
3. **Vector is the source of truth.** The `.svg` is committed and hand-edited. Every raster is generated and never hand-edited. If someone has to open Photoshop to change the icon, the pipeline is wrong.
4. **Different jobs get different artwork.** A menu-bar glyph is not the app icon at 18px. A 16px favicon is not the 512px one downscaled.
5. **Never reuse a previous project's palette, silhouette, or motif.** Each mark comes from *this* project's meanings. If two icons you made look related, one of them is wrong.

## Step 1 — Interrogate before drawing

Never start from "make an icon for X." Get four answers first — ask the user if they're present, otherwise dig them out of the repo (`README`, the `description` field in `package.json` / `Cargo.toml` / `pyproject.toml`, the actual entry point):

| Question | What it decides |
|---|---|
| What does it **do**, in verbs? | Half the metaphor |
| What is it **named** after, and why? | The other half |
| Where does it **appear**? | Menu bar, Dock, browser tab, and home screen need different artwork |
| What is the **smallest** place it appears? | The detail budget for everything |

That last one governs everything downstream. An icon that only ever appears at 512px can carry fine linework. One that lives in a 16px favicon can carry roughly three shapes.

## Step 2 — Find the double meaning

Do this **in writing, before drawing anything.** Two columns:

- **Function** — what the thing does, in plain verbs. *Small input, big output. Watches for changes. Routes between things. Remembers what you forgot.*
- **Identity** — the name, its origin, the reference, the in-joke, the pun, the feeling.

Now hunt for a **single physical object or gesture that lands on both lists.** That intersection is the mark.

Worked example — Reze, a text expander named after a Chainsaw Man character:

- *Function*: type two words, get a paragraph. **Small input, big output.**
- *Identity*: Reze is the Bomb Devil — pin in the neck.
- *Intersection*: **a pulled grenade pin.** Pull a tiny pin, get an explosion. It references the character and describes the app in one object.

**The test:** state the mark in one sentence that names both meanings. *"A bomb wearing a pulled pin — it nods at the Bomb Devil, and it's literally what the app does."* If your sentence only covers one meaning, you have a pictogram (function only) or fan art (identity only). Both are forgettable. Keep hunting.

Some ways through when the intersection isn't obvious:

- **No reference to work with** (a plain utility, a coined name): source the second meaning from the *feeling* the tool produces — relief, speed, precision, quiet — or find a structural pun in the letterform.
- **The obvious object is taken** (a shield for security, a rocket for launch): go one level more specific. Not "a shield" but "a shield with the boss's dents already in it."
- **Genuinely only one meaning available**: then make that one meaning unusually specific. Not "a database" but "a stack of drawers with exactly one pulled open." Specificity is the fallback for duality.

Do not proceed to drawing until this sentence exists.

## Step 3 — Silhouette before color

Fill the entire concept solid black and look at it at 32px.

- Still recognizable? Proceed.
- Reads as a blob? **Fix the shape.** No gradient, glow, or bevel has ever rescued a silhouette that doesn't work. This is where most icons are actually lost, and it's cheap to fix here and expensive to fix later.

Two more checks that cost nothing: squint at it, and view it in grayscale. If the subject and the background collapse into one value, the icon depends on hue alone and will disappear for a meaningful fraction of viewers and on a meaningful fraction of displays.

## Step 4 — Choose the canvas per target

Full tables in `references/platform-specs.md`. The rules that matter here:

- **Author at the largest target** so generation only ever downscales. For a desktop app that is 1024×1024.
- **Bake in the platform's grid where the platform has one.** macOS expects the artwork inset inside a squircle — put that in the source (824×824 body on a 1024 canvas, corner radius 185) so the generator never has to guess at padding.
- **Never bake in a grid the platform doesn't want.** iOS icons are full-bleed with square corners; the system does the masking. A pre-rounded iOS icon gets rounded twice and looks visibly wrong.
- **Maskable / adaptive targets need a safe zone.** Anything outside the center circle can and will be cropped by the launcher.

## Step 5 — Draw with the craft rules

Full set in `references/craft-rules.md`. The ones that do the most work:

- **One light source, upper left, consistent across every element.** Mixed lighting is the most common tell of an amateur icon.
- **Self-shade with an off-center radial gradient** rather than painting a highlight shape on top. A body filled with a radial gradient whose center sits at roughly `(0.34, 0.28)` shades itself and reads as a real object.
- **Two hues maximum, plus neutrals.** Rainbow gradients read cheap. Keep gradient stops few and the hue travel short.
- **Fewer, bigger shapes.** Five distinct elements is a lot. One focal element should occupy 55–70% of the safe area.
- **Punch details out with masks, don't paint them in the background color.** A grin cut out of a shape stays correct on any background and under any system tint; a grin painted in the background color breaks the moment either changes. This is mandatory for template images.
- **No text, no hairlines, no drop shadows.** Letterforms are illegible below ~64px, hairlines vanish, shadows turn to mud.
- **Optical centering beats mathematical centering.** A mark with visual weight low in the frame needs to sit slightly high to look centered.

## Step 6 — Cut optical sizes

A 1024px illustration rendered at 16px is mud no matter how good the renderer is. **This is not a rendering problem and no tool fixes it.**

Ship a simplified variant for small targets (≤32px, sometimes ≤48px):

- Delete incidental detail — sparks, texture, secondary elements
- Thicken every stroke relative to the canvas
- Raise contrast between the focal element and the background
- Enlarge the focal element within the frame, trimming margin

This is the single clearest difference between a professional icon set and a resized one. Keep it as a second SVG (`icon-small.svg`) and route the small sizes through it in the generation script.

## Step 7 — Separate artwork for separate jobs

| Job | Needs its own file because |
|---|---|
| App icon (Dock, Finder, home screen) | Full colour, full detail, platform grid baked in |
| macOS menu-bar / tray glyph | Must be a **template image**: pure black + alpha, nothing else. The system tints the alpha channel, so it comes out white on a dark menu bar and black on a light one automatically. **Drawing it white breaks this** — it will be invisible in light mode |
| Windows / Linux tray | No template concept — these need the *colour* icon; a black glyph disappears into a dark taskbar |
| Favicon at 16px | Needs the optical-size variant from step 6 |
| Maskable / Android adaptive | Needs the safe-zone layout, and its own background layer |

The macOS template rule is counterintuitive enough to state twice: **black plus alpha, never white.** Author it at 4× the final size (a menu-bar icon renders around 16–18pt tall, so author at 64–72px) with deliberately fat strokes, because thin strokes disintegrate in that downscale.

## Step 8 — Generate

Commands and per-framework recipes in `references/pipeline.md`. Two rules:

- **Render every size from the SVG.** Never downscale one PNG to produce another — resampling a raster loses the crispness that rendering from vector preserves at each target.
- **Make it one command.** Add an `icons` script to the project so regenerating is `npm run icons` (or the equivalent). Anything requiring remembered manual steps rots immediately.

## Step 9 — Verify before declaring done

- [ ] Rendered at 16, 32, and 128 and **actually looked at each** — not assumed
- [ ] Checked on a light background, a dark background, and a busy wallpaper
- [ ] Menu-bar glyph verified in both light and dark mode
- [ ] Silhouette and grayscale still readable
- [ ] Every generated raster is reproducible from the committed SVG by one command
- [ ] No sourced or traced third-party artwork anywhere in the tree
- [ ] The double-meaning sentence from step 2 is written down — in the SVG's header comment, so the next person understands the mark

That last one matters more than it looks. Put a comment at the top of the SVG explaining what the mark means and why the numbers are what they are. It is the difference between an icon someone can maintain and an icon nobody dares touch.

## Anti-patterns — do not ship these

Stock ideas that signal no thought happened: a gradient blob with a lightning bolt; a generic rocket for "launch"; a brain wired with circuit traces for anything AI; a hexagon with a letter in it; a bare chat bubble; a magnifying glass over nothing; a gear for "settings" as the *app's own* identity.

Craft failures: text or initials as the whole mark; photographic realism; more than two hues; hairline strokes; drop shadows; a bevel-and-emboss treatment; the same artwork stretched across every size; a white menu-bar glyph on macOS; an iOS icon with the corners pre-rounded.
