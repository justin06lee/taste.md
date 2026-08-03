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

The research step is the whole thing, and it comes before any drawing. If the project is named after anything — a character, a technique, a place, a piece of jargon — the skill **searches for it first**: what it is, who introduced it, what physical objects attach to it, what a fan would name first. Then it quotes a concrete detail from that research.

It does not invent a metaphor. Inventing feels faster and produces a plausible abstraction roughly nine times out of ten.

| Project | Reference | Detail quoted | Also describes the tool? |
|---|---|---|---|
| **reze** | Reze, the Bomb Devil | The pull-pin in her neck | Yes — pull a tiny pin, get an explosion |
| **shoko.md** | Shoko Ieiri, the chain-smoker | An ashtray overflowing with butts | No, and it does not matter |
| **caveira** | Caveira, the R6 operator | Her silencing gesture with the knife | Loosely |
| **bmo** | BMO from Adventure Time | BMO, standing in grass | No |

Three of those four are pure quotation with no functional pun at all, and they land because the detail is unmistakable. Double-coding — where the quoted detail *also* says something about the software — is a bonus found inside the research, never a substitute for it.

The failure mode it exists to prevent is seductive: an object that is a clever metaphor for the software but quotes nothing real. If the explanation is a pun rather than a citation, the mark goes back to step 1.

The image at the top of this README is the rule applied to itself. It's Remy mid-taste, eyes shut, ringed by the synesthesia bursts Michael Gagné animated for *Ratatouille* — yellow rings of light for cheese, pink spirals for strawberry. The cool grey against warm bursts is the film's own palette logic: Brad Bird's team made the rat world cool and the human world warm, so Remy always reads as outside the warmth he wants into. Every one of those decisions came from research, not invention. The first attempt at this mark was a tasting spoon on its own — a pun, quoting nothing, and forgettable.

## Part 2 — READMEs

The shape, in one line: **a centered mark, the name, a one-line tagline, a rule, then prose.** Nothing else above the fold.

The skill settles the header image before writing anything — it looks for an existing asset in the repo, and if there isn't one it **asks whether you already have a logo** rather than inventing one silently. Only if you say so does it route into Part 1 and design a mark.

**A README header image is not an app icon**, and treating it like one is what makes a page look like a product listing. No squircle, no tile, no border, no gradient panel — those constraints exist so an icon sits correctly in a Dock, and there is no Dock on a README. Draw the subject free-form on transparency, at whatever proportions it wants: `bmo.png` is 1007×875.

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
- **Few shapes, always.** Hand-authored SVG has a hard detail ceiling. Aim for the fewest shapes that make the subject unmistakable, then remove one more. If the subject genuinely needs painterly richness, the skill says so and asks — rather than grinding out a stiff vector approximation.
- **Vector is the source of truth.** The SVG is committed and hand-edited; every raster is generated and never touched by hand.
- **Render each size from the SVG.** Never downscale a PNG to make another PNG.
- **Different jobs get different files.** A menu-bar glyph is not the app icon at 18px.
- **No decoration that carries no information.** No badges, no emoji, no ornament for its own sake.
- **No palette reuse across projects.** Each mark comes from that project's own meanings.

## License

MIT — see [LICENSE](LICENSE).
