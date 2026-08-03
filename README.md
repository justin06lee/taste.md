<div align="center">

<img src="assets/taste.png" alt="taste.md" width="300" />

# taste.md

**A Claude Code skill for how a project looks to whoever finds it.**<br>
*Original marks that mean two things at once, and READMEs that read as finished.*

</div>

---

[taste.md](https://github.com/justin06lee/taste.md) covers the two surfaces a project is judged on before anyone runs it: its **icon** and its **README**. It refuses to source existing artwork, forces a double-meaning metaphor before anything gets drawn, produces every size every platform asks for, and lays out a README in the house style — centered mark, name, one-line tagline, then prose. No badges. No emoji.

## Install

With [bmo](https://github.com/justin06lee/bmo):

```bash
bmo add justin06lee/taste.md
```

Or from a local clone:

```bash
bmo add ./taste.md        # everywhere (global)
bmo add ./taste.md here   # just this project
```

Installs as the `taste` skill (`/taste` in Claude Code). It also triggers on its own — "make an app icon", "I need a favicon", "the tray icon looks bad at small sizes", "write a README", "make this repo look good before I make it public".

## Part 1 — Icons

Icons come out generic for three reasons, in order of how often they happen:

| Failure | What it looks like | What the skill does |
|---|---|---|
| **Stock idea** | A gradient blob with a lightning bolt | Forces a written double-meaning metaphor before drawing |
| **Wrong canvas** | Crisp at 512, mud at 16 | Author at the largest target, bake in the platform grid, cut optical-size variants |
| **One file, many jobs** | A white menu-bar glyph that vanishes in light mode | Separate artwork per job, with the template-image rules spelled out |

The metaphor step is the whole thing. Before anything is drawn, two lists get written down — **function** (what it does, in plain verbs) and **identity** (the name, its origin, the joke) — then a single object that lands on **both**.

> **Worked example.** Reze, a text expander named for a Chainsaw Man character.
> *Function:* type two words, get a paragraph — small input, big output.
> *Identity:* Reze is the Bomb Devil, pin in the neck.
> *Intersection:* **a pulled grenade pin.** Pull a tiny pin, get an explosion.
> It references the character and describes the app in one object.

The test is a single sentence naming both meanings. Cover only one and you have a pictogram (function alone) or fan art (identity alone). Both are forgettable.

The mark at the top of this README came from the same steps: a tasting spoon, because you judge a whole dish from one small spoonful the way you judge a whole app from one small icon.

## Part 2 — READMEs

The shape, in one line: **a centered mark, the name, a one-line tagline, a rule, then prose.** Nothing else above the fold.

The skill settles the header image before writing anything — it looks for an existing asset in the repo, and if there isn't one it **asks whether you already have a logo** rather than inventing one silently. Only if you say so does it route into Part 1 and design a mark.

Then: section order (what it does → install → quick start → reference → development → limitations → license), sentence-case headings, tables for anything shaped name-to-meaning, language tags on every fenced block, and a real `LICENSE` file — because a public repo without one is default copyright, and nobody may legally use or fork it whatever the README claims.

**Banned outright:** badges and emoji. Badges are noise that rots the moment a number changes and pushes the explanation below the fold. Emoji make headings harder to scan and break anchor links. Also out: centered body text, hand-written tables of contents, "Made with love" boilerplate, screenshots of terminal output, and marketing adjectives.

## Layout

Reference files load on demand, so the skill stays cheap until it's doing the work.

```
SKILL.md                        both procedures
references/platform-specs.md    every icon size, format, and grid rule
references/craft-rules.md       form, colour, light, legibility
references/pipeline.md          render commands and framework wiring
references/readme-style.md      header blocks, section order, full template
```

| Area | Contents |
|---|---|
| **Icon process** | Interrogate → double meaning → silhouette → canvas → draw → optical sizes → per-job artwork → generate → verify |
| **Platforms** | macOS (`.icns`, grid, menu-bar templates), iOS, Android adaptive, Windows (`.ico`, tiles, tray), Linux (hicolor, tray), web (favicon set, manifest, maskable, `apple-touch-icon`) |
| **Craft** | Optical centering, colour and gradient discipline, one light source, self-shading, legibility tests, small-size adaptation |
| **Pipeline** | `rsvg-convert` / `resvg` / `iconutil` / `icotool` / `oxipng` recipes, Tauri and Electron wiring, what to commit |
| **README** | Header block variants, image conventions, section order, prose and markdown rules, license footer, the ban list |

## House rules it enforces

- **Never source, always invent.** No downloaded, traced, or embedded third-party artwork — it is someone else's copyright in your repo, and it caps the work at retrieval.
- **Vector is the source of truth.** The SVG is committed and hand-edited; every raster is generated and never touched by hand.
- **Render each size from the SVG.** Never downscale a PNG to make another PNG.
- **Different jobs get different files.** A menu-bar glyph is not the app icon at 18px.
- **No decoration that carries no information.** No badges, no emoji, no ornament for its own sake.
- **No palette reuse across projects.** Each mark comes from that project's own meanings.

## License

MIT — see [LICENSE](LICENSE).
