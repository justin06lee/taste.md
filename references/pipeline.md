# Generation pipeline

SVG in, every platform raster out, by one command.

---

## The two rules

**Render every size from the SVG.** Never downscale one PNG to produce another.
Resampling a raster softens edges that rendering from vector keeps crisp at each
target size. `sips`, `convert -resize`, and every "resize my icon" web tool get
this wrong by construction.

**Make it one command.** Put an `icons` script in the project. If regenerating
requires remembered manual steps, the rasters and the SVG drift apart within a
month and nobody notices until the icon looks wrong in a release.

## Tools

| Tool | Install | Use for |
|---|---|---|
| `rsvg-convert` | `brew install librsvg` | SVG → PNG. The default choice: fast, accurate, correct gradient and mask handling |
| `resvg` | `cargo install resvg` | SVG → PNG. Excellent alternative, very strong spec compliance |
| `inkscape` | `brew install --cask inkscape` | SVG → PNG when the file uses exotic features the others miss |
| `iconutil` | built into macOS | `.iconset` → `.icns` |
| `icotool` | `brew install icoutils` | PNGs → multi-resolution `.ico` |
| `magick` | `brew install imagemagick` | PNG → `.ico`, and PNG manipulation |
| `oxipng` | `brew install oxipng` | Lossless PNG size reduction |
| `svgo` | `npm i -g svgo` | SVG cleanup before committing |

**Do not rasterize SVG with ImageMagick.** `magick` only renders SVG properly if
it was built with the librsvg delegate; otherwise it silently falls back to its
weak internal MSVG renderer and mangles gradients and masks. Check with
`magick -list delegate | grep -i svg`. Rendering with `rsvg-convert` and using
`magick` only for PNG→ICO avoids the question entirely.

## Recipes

### SVG → PNG at a size

```bash
rsvg-convert -w 1024 -h 1024 assets/icon.svg -o build/icon-1024.png
```

### PNGs → macOS `.icns`

```bash
mkdir -p build/icon.iconset
for s in 16 32 128 256 512; do
  rsvg-convert -w $s        -h $s        assets/icon.svg -o build/icon.iconset/icon_${s}x${s}.png
  rsvg-convert -w $((s*2))  -h $((s*2))  assets/icon.svg -o build/icon.iconset/icon_${s}x${s}@2x.png
done
iconutil -c icns build/icon.iconset -o build/icon.icns
```

Route 16 and 32 through `icon-small.svg` if an optical-size variant exists.

### PNGs → Windows `.ico`

```bash
for s in 16 24 32 48 64 128 256; do
  rsvg-convert -w $s -h $s assets/icon.svg -o build/ico-$s.png
done
magick build/ico-16.png build/ico-24.png build/ico-32.png build/ico-48.png \
       build/ico-64.png build/ico-128.png build/ico-256.png build/icon.ico
```

`icotool -c -o icon.ico ico-*.png` works equally well.

### Full web favicon set

```bash
# Vector favicon — cleaned, not minified into illegibility
svgo assets/favicon.svg -o public/favicon.svg

# Legacy multi-res .ico, small sizes from the optical variant
rsvg-convert -w 16 -h 16 assets/icon-small.svg -o /tmp/f16.png
rsvg-convert -w 32 -h 32 assets/icon-small.svg -o /tmp/f32.png
rsvg-convert -w 48 -h 48 assets/icon.svg       -o /tmp/f48.png
magick /tmp/f16.png /tmp/f32.png /tmp/f48.png public/favicon.ico

# apple-touch-icon — MUST be opaque; iOS composites alpha onto black
rsvg-convert -w 180 -h 180 -b '#14110f' assets/icon.svg -o public/apple-touch-icon.png

# Manifest icons
rsvg-convert -w 192 -h 192 assets/icon.svg          -o public/icon-192.png
rsvg-convert -w 512 -h 512 assets/icon.svg          -o public/icon-512.png
rsvg-convert -w 512 -h 512 assets/icon-maskable.svg -o public/icon-maskable-512.png
```

`-b '#14110f'` sets a background colour, which is how you flatten alpha for
`apple-touch-icon`.

### Tauri v2

```json
"scripts": {
  "icons": "rsvg-convert -w 1024 -h 1024 assets/icon.svg -o assets/icon-1024.png && rsvg-convert -w 72 -h 72 assets/tray.svg -o src-tauri/icons/tray.png && tauri icon assets/icon-1024.png && rm -rf src-tauri/icons/ios src-tauri/icons/android"
}
```

`tauri icon` produces `.icns`, `.ico`, and every PNG. The `rm -rf` drops the
mobile sets for a desktop-only app. The tray glyph renders separately from its
own SVG — `tauri icon` does not make one, and the app icon is wrong for the job.

Embed the tray PNG in the binary so it cannot go missing at runtime:

```rust
#[cfg(target_os = "macos")]
let tray_icon = Image::from_bytes(include_bytes!("../icons/tray.png"))?;
#[cfg(not(target_os = "macos"))]
let tray_icon = Image::from_bytes(include_bytes!("../icons/32x32.png"))?;

TrayIconBuilder::new()
    .icon(tray_icon)
    .icon_as_template(cfg!(target_os = "macos"))
```

Only macOS has template images. Elsewhere a pure-black glyph disappears into a
dark panel, so those platforms get the colour icon.

### Optimization

```bash
oxipng -o 4 --strip safe public/*.png src-tauri/icons/*.png
```

Lossless, and typically 20–40% off. Safe to run on every generated PNG as the
last step of the script.

## Committing

| Path | Committed? |
|---|---|
| `assets/*.svg` | **Yes** — the source of truth |
| Generated rasters consumed by a build | Yes, if the build expects them present (Tauri, Electron) |
| Generated rasters in a web `public/` dir | Either; if generated in CI, gitignore them |
| Intermediate `build/` or `.iconset/` | No — gitignore |

Whatever the choice: **generated rasters are never hand-edited.** Put that in
the README next to the `icons` script, because it is exactly the rule someone
breaks in a hurry.

## Verifying

```bash
sips -g pixelWidth -g pixelHeight public/icon-512.png   # confirm dimensions
magick identify public/favicon.ico                       # list every .ico entry
iconutil -l build/icon.icns                              # list .icns entries
```

Then actually look at the thing. Open the 16px render at 100% zoom. Drop the app
in the Dock. Load the favicon in a real tab. Switch the system between light and
dark mode and check the tray glyph in both.

Every rule in this skill exists because a step got skipped and the result looked
fine in the editor and wrong on the machine.
