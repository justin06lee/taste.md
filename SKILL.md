---
name: taste
description: Use when shaping how a project looks to whoever encounters it — its icon or its README. Covers app icons, favicons, menu-bar/tray/taskbar glyphs, maskable PWA icons, .icns/.ico/.iconset bundles and full multi-size sets; and README design — the centered header block with a mark, section order, markdown conventions, and the license footer. Triggers on "make an icon", "app icon", "logo for my app", "favicon", "tray icon", "generate all the icon sizes", "my icon looks generic/blurry at small sizes", "write a README", "make the README look good", "my README is ugly", "add a header image", and on preparing a repo to go public. Enforces original artwork, no badges, no emoji.
---

# taste

How a project looks to whoever finds it: the mark, and the page that mark sits on top of.

Two jobs, sharing one set of principles:

| Job | Go to |
|---|---|
| An icon, favicon, tray glyph, or full multi-size set | **Part 1** |
| A README — header block, structure, style | **Part 2** |

A README needs a mark, so Part 2 routes into Part 1 when there isn't one yet. Doing an icon alone is common; doing a README without an image is not — the header image is the whole reason these pages look finished.

## Shared non-negotiables

- **Invent, never source.** No downloaded, traced, or embedded third-party artwork. It is someone else's copyright in the user's repo, and it caps the work at *retrieval* when invention has a far higher ceiling.
- **Vector is the source of truth.** SVG committed and hand-edited; every raster generated and never hand-edited.
- **No decoration that carries no information.** No badges, no emoji, no ornament for its own sake. If it isn't telling the reader something, it is in the way.
- **Never reuse a previous project's palette, silhouette, or motif.** Each project gets its own.

---

# Part 1 — Icons and marks

Invent an original mark that means two things at once, then render every platform size from a single committed SVG.

Bad icons fail for three reasons, in this order of frequency:

1. The idea is stock — a gradient blob with a lightning bolt through it.
2. The artwork was authored at the wrong size and downscaled into mud.
3. One file was reused for jobs that need genuinely different files.

Every step below exists to prevent one of those. Work them in order; step 2 is the one that produces the reaction *"how did you even do that"*, and it is the one most likely to get skipped.

## Rules specific to icons

1. **The mark carries two meanings.** See step 2. This is the whole of Part 1.
2. **Different jobs get different artwork.** A menu-bar glyph is not the app icon at 18px. A 16px favicon is not the 512px one downscaled.
3. **When the user offers a reference** ("make it look like X"), treat it as a source of **meaning**, never a source of pixels. Riff on what it means; draw it yourself.
4. **If someone has to open Photoshop to change the icon, the pipeline is wrong.** One command regenerates everything from the SVG.

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

---

# Part 2 — READMEs

The README is the whole product for anyone who hasn't run it yet. Full style guide, copy-paste header blocks, and a complete template in `references/readme-style.md` — read it before writing.

The shape, in one picture: **a centered mark, the name, a one-line tagline, a rule, then prose.** Nothing else above the fold. That opening is what makes these pages read as finished rather than as notes.

## Step 1 — Settle the header image first

Never write the README around a missing image and never generate one silently. In order:

1. **Look for one already in the repo** — `assets/*.png|svg`, `docs/*.png`, `<name>.png` at the root, anything matching `*logo*` or `*readme*`. If it exists, use it.
2. **If there is none, ask the user.** Verbatim intent: *"Do you already have a logo or image for the header, or should I make one?"* They may have a mark from elsewhere, a commissioned asset, or a strong opinion. Asking costs one turn; guessing wrong wastes the whole design.
3. **Only if they say make one, go to Part 1** and design an original mark, then render a PNG for the README.

**Render a PNG for the README even though the SVG is the source.** GitHub proxies and sanitizes SVGs, and complex ones — gradients, masks, clip paths — can render wrong or not at all. Commit the SVG as source and a rendered PNG at 2× the display width.

## Step 2 — Build the header block

Two variants in `references/readme-style.md`; both are in house use, pick per repo. Display width 300–360 for most marks, 180 for a tall or dense one.

One trap worth stating here because it silently breaks the page: **markdown inside an HTML block only parses when blank lines separate it from the tags.** Without a blank line before `# name` inside a `<div align="center">`, GitHub renders a literal `# name`. If the title comes out as visible hash marks, that is why.

## Step 3 — Write the opening paragraph

Straight after the rule, with no heading above it. One to three sentences expanding the tagline into what the thing does and who it's for. This is the paragraph people actually read.

Lead with the problem when the problem isn't self-evident, and **name the tradeoff** — *"no LLM-as-judge, just exact checks"*, *"only timestamps change"*. Stating a boundary builds more trust than listing features.

## Step 4 — Order the sections

What it does → Install → Quick start → Reference (commands, flags, configuration, architecture) → Development → Notes and limitations → **License, last, always**.

Skip what doesn't apply. Don't reorder what does.

## Step 5 — Apply the markdown conventions

Sentence-case headings, consistent throughout. Tables for anything shaped name-to-meaning — they compress better than bullets and stay scannable. Fenced blocks always carry a language tag, with comments on the non-obvious lines *inside* the block. Stop at `###`; deeper nesting means it should have been its own document.

## Step 6 — Close with the license

```markdown
## License

MIT — see [LICENSE](LICENSE).
```

And an actual `LICENSE` file at the repo root. **A public repo without one is default copyright** — nobody may legally use, fork, or contribute, whatever the README says. GitHub detects the file and surfaces the license in the sidebar; that detection is what makes it real to anyone browsing. Check for it whenever a repo is going public.

## Banned in READMEs

**Badges.** No shields.io, no build-status pills, no vanity counters. They are noise, they rot the moment a number changes, and a stack of them pushes the actual explanation below the fold.

**Emoji.** Not in headings, not in body text, not as bullets. They make headings harder to scan, break anchor links, and read as decoration where the words should be doing the work.

Also out: centered body text (only the header block is centered); a hand-written table of contents (GitHub generates one from the headings); "Made with love" and "Contributions welcome!" boilerplate; screenshots of terminal output where a code block would be searchable and copyable; marketing adjectives like *blazingly fast* and *powerful* — state the mechanism and let the reader conclude it.

---

## Reference files

| File | Contents |
|---|---|
| `references/platform-specs.md` | Every icon size, format, and grid rule, per platform |
| `references/craft-rules.md` | Form, colour, light, legibility, small-size adaptation |
| `references/pipeline.md` | Render commands, framework wiring, what to commit |
| `references/readme-style.md` | Header blocks, section order, markdown conventions, full template |
