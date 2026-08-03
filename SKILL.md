---
name: taste
description: Use when shaping how a project looks to whoever encounters it — its icon or its README. Covers app icons, favicons, menu-bar/tray/taskbar glyphs, maskable PWA icons, .icns/.ico/.iconset bundles and full multi-size sets; and README design — the centered header block with a mark, section order, markdown conventions, and the license footer. Triggers on "make an icon", "app icon", "logo for my app", "favicon", "tray icon", "generate all the icon sizes", "my icon looks generic/blurry at small sizes", "write a README", "make the README look good", "my README is ugly", "add a header image", and on preparing a repo to go public. Researches whatever the project is named after and quotes a concrete detail from it rather than inventing a metaphor. Enforces original artwork, no badges, no emoji.
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

Every step below exists to prevent one of those. Work them in order. Step 1 is the one that produces the reaction *"how did you even do that"* — and it is the one most likely to get skipped, because inventing feels faster than looking things up. It isn't; it just fails later.

## Rules specific to icons

1. **Quote a researched detail; do not invent a metaphor.** See steps 1 and 2. This is the whole of Part 1, and the failure mode it prevents is the most common one by far.
2. **Few shapes, always.** Hand-authored SVG has a hard detail ceiling — you are placing coordinates, not painting. Aim for the fewest shapes that make the subject unmistakable, then remove one more. Detail that would be cheap in a rendered illustration comes out stiff and fussy here.
3. **Different jobs get different artwork.** A menu-bar glyph is not the app icon at 18px. A 16px favicon is not the 512px one downscaled.
4. **When the user offers a reference** ("it's named after X"), that reference is the *subject*, not a mood board. Go find out what X actually looks like, then draw that yourself.
5. **If someone has to open Photoshop to change the icon, the pipeline is wrong.** One command regenerates everything from the SVG.

## Step 1 — Research the reference before drawing anything

**This step is not optional and it comes first.** Working from a name plus imagination produces a plausible abstraction roughly nine times out of ten. Working from a specific, researched, concrete detail produces something people react to.

If the project name points at *anything* external — a character, a technique, an operator, a myth, a place, a piece of domain jargon — **search the web for it before drawing.** Multiple sources; do not stop at the first summary.

What you are looking for, in priority order:

| Find out | Because |
|---|---|
| **What is the single most recognizable thing about it?** | This is almost always the mark. What would a fan name first? |
| **Who or what introduced it, and in what scene?** | Specific moments give you specific objects — a weapon, a stance, a prop |
| **What physical objects are attached to it?** | Objects draw; concepts don't |
| **What are the rules of its world?** | Sometimes the mechanic is more visual than the character |
| **What does it actually look like?** | Colour, shape, silhouette — get this from sources, not from guessing |

Then name the detail you're quoting and where you got it. If you cannot point at a source, you are inventing again.

> **Worked example — `simpledomain`.** The name is the Jujutsu Kaisen technique. Searching turns up: it is a *simplified* domain, a defensive barrier that neutralizes an enemy's domain; it is introduced by **Kasumi Miwa**, a sword user, whose version is called *Shin: Ryuiki* and is drawn as a circular barrier struck out from her katana draw. That research yields concrete candidates — the katana draw, the barrier ring at the moment of the cut, Miwa's stance — instead of the abstraction "a circle meaning containment."

**When the name has no external referent** (a coined word, a plain English noun), do not fall back on a conceptual pun — that is exactly the failure mode. Instead, research the *domain the tool works in* and quote a specific real object from it: the actual instrument practitioners use, the actual artifact the work produces. A specific real object always beats a clever abstract one.

Two practical questions to settle in the same pass, since they constrain everything downstream:

| Question | What it decides |
|---|---|
| Where does it **appear**? | Menu bar, Dock, browser tab, home screen, and README header all need different artwork |
| What is the **smallest** place it appears? | The detail budget for the whole design |

An icon that only ever appears at 512px can carry fine linework. One that lives in a 16px favicon can carry about three shapes. A README header image has no upper bound and no shape constraint at all — see Part 2.

## Step 2 — Pick the detail, then check whether it double-codes

From the research, list the concrete candidates and rank them on two things:

1. **How immediately someone who knows the reference would recognize it.**
2. **How well it reduces.** Can you draw it in five or six shapes and still have it read? A single object with a strong outline reduces beautifully. A character in a pose, holding a thing, in a scene does not — that is an illustration, and hand-authored SVG will make it stiff.

Take the candidate that scores on both. When they conflict, **the reducible one usually wins**, because a clean simple mark beats a fussy accurate one at every size it will actually be seen at.

> Reze's pin is the model: one object, a handful of shapes, unmistakable. The Remy mark on this repo is the honest counterweight — the research is right and it is likeable, but a head plus an arm plus a spoon plus scattered bursts is a scene, and it carries more detail than hand-written SVG does gracefully. Given the same research, the pin-equivalent would have been the bursts alone, or the spoon with the bursts blooming off it.

Then — and only then — check whether the detail *also* says something about what the software does. If it does, that is a bonus worth having. **If it doesn't, ship it anyway.** Recognition beats cleverness.

The evidence for that ordering, from marks that landed:

| Project | Reference | Detail quoted | Also describes the tool? |
|---|---|---|---|
| **reze** | Reze, the Bomb Devil | The pull-pin in her neck | Yes — pull a tiny pin, get an explosion |
| **shoko.md** | Shoko Ieiri, the chain-smoker | An ashtray overflowing with butts | No, and it does not matter |
| **caveira** | Caveira, the R6 operator | Her silencing gesture with the knife | Loosely |
| **bmo** | BMO from Adventure Time | BMO, standing in grass | No |

Three of those four are pure quotation with no functional pun at all, and they work because the detail is unmistakable. The one that double-codes is better still — but the double meaning was found *inside* the research, not substituted for it.

**The failure mode, stated plainly, because it is seductive:** inventing an object that is a clever metaphor for the software but quotes nothing real. A tasting spoon for a skill called "taste." A cursor inside a ring for a tool that contains focus. Both are legible, both are competent, and both are forgettable, because there is nothing behind them to recognize. If your mark's explanation is a pun rather than a citation, go back to step 1.

Write the sentence before drawing: **"It's <the detail>, from <the source>."** Add "— which also happens to be what the app does" only if that is genuinely true.

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
- [ ] The citation sentence from step 2 is written down — in the SVG's header comment, naming the detail and its source, so the next person understands the mark

That last one matters more than it looks. Put a comment at the top of the SVG explaining what the mark quotes, where it came from, and why the numbers are what they are. It is the difference between an icon someone can maintain and an icon nobody dares touch.

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
3. **Only if they say make one, go to Part 1** — the research step especially — and design from a quoted detail, then render a PNG for the README.

**A README header image is not an app icon.** This is the most common mistake and it makes the page look like a product listing instead of a project:

| | App icon | README header image |
|---|---|---|
| Shape | Rounded-square grid, mandatory | **Anything.** Free-form, and often not square at all |
| Background | Fills the tile | **Transparent.** It sits on the page, not in a box |
| Framing | Squircle, margins, safe zone | None. No tile, no border, no grid |
| Detail budget | Must survive 16px | Displays at ~300px and never shrinks — carry as much detail as it deserves |
| Subject | A reduced mark | The referenced thing, depicted properly |

The house examples: `bmo.png` is BMO standing in grass with bees, 1007×875 with alpha — not square, no tile. `shoko.png` is an overflowing ashtray. `caveira.png` is the operator's silencing gesture. None of them are wrapped in a rounded rectangle, because none of them are icons.

So: draw the subject on transparency, sized to its own natural proportions. Do not add a tile, a squircle, a gradient panel, or a border — those exist to make an icon sit correctly in a Dock, and there is no Dock here.

**Render a PNG even though the SVG is the source.** GitHub proxies and sanitizes SVGs, and complex ones — gradients, masks, clip paths — can render wrong or not at all. Commit the SVG as source and a PNG at 2× the display width. Observed house sizes: 480×480 is the common case, up to ~1250 for a detailed one.

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
