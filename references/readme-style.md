# README style

The house style, extracted from `bmo`, `shoko.md`, `caveira`, `stockpile`,
`omniscience.md`, `zeroforce`, and `taste.md`. Copy-paste blocks at the bottom.

---

## The header block

Every good README in this style opens the same way: a centered mark, the name,
a one-line tagline, a rule, then prose. Nothing else above the fold.

### Variant A — centered div with markdown inside

Best when the tagline wants links or bold. Used by `shoko.md`, `omniscience.md`,
`monkeyclaw`.

```html
<div align="center">

<img src="assets/name.png" alt="name" width="330" />

# name

**One bold line saying what it is.**<br>
*One italic line saying the thing that makes it different.*

</div>

---
```

**The gotcha:** markdown inside an HTML block is only parsed if blank lines
separate it from the tags. Without the blank line before `# name`, GitHub
renders a literal `# name`. This bites every time someone tightens the
whitespace.

### Variant B — pure HTML

No blank-line trap, and exact control over line breaks. Used by `bmo`,
`caveira`, `stockpile`.

```html
<p align="center">
  <img src="name.png" alt="name" width="360">
</p>

<h1 align="center">name</h1>

<p align="center">
  <em>One line saying what it is —<br/>
  and the clause that makes it different.</em>
</p>

---
```

### Light and dark variants

When the mark needs to flip with the reader's theme, use `<picture>`:

```html
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/name-dark.png">
  <source media="(prefers-color-scheme: light)" srcset="assets/name-light.png">
  <img alt="name" src="assets/name-light.png" width="320">
</picture>
```

A mark on a self-contained dark tile (the icon-style squircle) needs no variants
— it reads on both themes as-is. Only flat marks that sit directly on the page
background need the pair.

### Image conventions

| Decision | House answer |
|---|---|
| Location | `assets/name.png`, `docs/name.png`, or `name.png` at the repo root — all three are in use; pick one and be consistent |
| Format | **PNG.** GitHub proxies and sanitizes SVG; complex gradients and masks can render wrong or not at all. Keep the SVG as source, commit a rendered PNG for the README |
| Display width | 300–360 for most marks; 180 for a tall or very dense one |
| Render at | 2× the display width, so it stays sharp on retina |

## Section order

The order these repos converge on. Skip what doesn't apply; don't reorder what
does.

1. **What it does** — sometimes "Overview". Always first.
2. **Install** — every path (package manager, from source, one-liner)
3. **Quick start** / **Usage** — the shortest route to it working
4. **Reference** — commands, flags, configuration, architecture, API. The bulk.
5. **Development** — tests, build, code conventions
6. **Notes and limitations** / **Status** / **Out of scope** — what it won't do
7. **License** — last, always

## Prose

**The opening paragraph goes right after the rule, with no heading.** One to
three sentences expanding the tagline into what the thing actually does and who
it's for. This is the paragraph people actually read.

**Lead with the problem when the problem isn't obvious.** `zeroforce` opens with
*"If a supplier fails today, how fast does the disruption cascade through the
dependency network?"* — the reader knows within one sentence whether they care.

**Name the tradeoff.** `shoko.md`: *"no LLM-as-judge, just exact checks."*
`caveira`: *"Author and committer identities are preserved exactly; only
timestamps change."* Stating the boundary builds more trust than listing
features.

**Wrap source lines at ~90 characters.** Diffs stay readable and reviewable.
Be consistent within a file.

## Markdown conventions

- **Sentence case headings** (`## What it does`), consistent throughout a file.
- **Tables for anything shaped name-to-meaning** — flags, capabilities, scopes,
  file layouts. They compress far better than bullet lists and stay scannable.
- **Fenced code blocks always carry a language tag.** Comment the non-obvious
  lines *inside* the block rather than explaining them after it:

  ```bash
  bmo add owner/repo here         # install into this project
  bmo add owner/repo everywhere   # install globally (the default)
  ```

- **Bold the term being defined**, not for shouting.
- **`---` rules to separate major zones**, sparingly — after the header block,
  and between top-level sections in long documents.
- **Relative links to files in the repo**: `[LICENSE](LICENSE)`,
  [`docs/RUNBOOK.md`](docs/RUNBOOK.md).
- **Blockquote for a worked example**, so it reads as illustration rather than
  instruction.
- **Stop at `###`.** Deeper nesting means the section should have been its own
  document.

## License section

Last section, every time:

```markdown
## License

MIT — see [LICENSE](LICENSE).
```

And an actual `LICENSE` file at the repo root. A public repo without one is
default copyright: nobody may legally use, fork, or contribute to it, whatever
the README says. GitHub detects the file and shows the license in the sidebar —
that detection is what makes it real to anyone browsing.

## Banned

**Badges.** No shields.io, no build-status pills, no vanity counters. They are
visual noise, they rot the moment a number changes, and a wall of them at the
top pushes the actual explanation below the fold. In this collection only
`monkeyclaw` carries them — seven, stacked — and it is the one header that looks
cluttered. That is the whole argument.

**Emoji.** Not in headings, not in body text, not as bullets. `monkeyclaw`'s
`## 🛡️ Why continuous testing` is the counter-example: it makes headings harder
to scan, breaks anchor links, and reads as decoration where the words should be
carrying it.

Also out:

| Don't | Why |
|---|---|
| Centered body text | Only the header block is centered; centered paragraphs are unreadable |
| A hand-written table of contents | GitHub renders one from the headings automatically |
| "Made with love", "Contributions welcome!" | Boilerplate nobody reads |
| Screenshots of terminal text | Use a code block — searchable, copyable, diffable |
| Marketing adjectives (*blazingly fast*, *powerful*) | State the mechanism instead and let the reader conclude it |
| A features list of things every tool has | Only list what distinguishes it |

---

## Template

```markdown
<div align="center">

<img src="assets/name.png" alt="name" width="330" />

# name

**What it is, in one bold line.**<br>
*What makes it different, in one italic line.*

</div>

---

[name](https://github.com/owner/name) does the thing, for these people, in this
way. One to three sentences. No heading above this paragraph.

## Install

```bash
one-liner
```

Or from source:

```bash
git clone https://github.com/owner/name
cd name && make
```

## Quick start

```bash
name do-the-thing          # the shortest path to it working
name do-the-thing --flag   # the one variation people immediately want
```

## What it does

| Capability | Behaviour |
|---|---|
| **Thing one** | What it does and the boundary it respects |
| **Thing two** | What it does and the boundary it respects |

## Configuration

...

## Development

```bash
make test
make build
```

## Notes and limitations

- What it deliberately does not do
- The condition under which it won't work

## License

MIT — see [LICENSE](LICENSE).
```
