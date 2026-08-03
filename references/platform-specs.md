# Platform specs

Every size, format, and grid rule, per target. Look up only the platforms the
project actually ships to.

---

## macOS — app icon

| Property | Value |
|---|---|
| Source canvas | 1024 × 1024 |
| Visible body | 824 × 824, centered (100px margin on all sides) |
| Corner radius | 185 (of the 824 body) |
| Format | `.icns` |
| Alpha | Yes — the margin is transparent |

Bake the squircle into the SVG source. The margin is not decoration: macOS
reserves it so icons of different shapes optically align in the Dock, and
system-drawn shadows have room to land.

The `.icns` is built from an `.iconset` folder with **exactly** these filenames:

```
icon_16x16.png        16      icon_128x128.png      128
icon_16x16@2x.png     32      icon_128x128@2x.png   256
icon_32x32.png        32      icon_256x256.png      256
icon_32x32@2x.png     64      icon_256x256@2x.png   512
icon_512x512.png      512     icon_512x512@2x.png   1024
```

Route 16 and 32 through the optical-size variant.

## macOS — menu bar (status item)

| Property | Value |
|---|---|
| Rendered height | ~16–18pt (menu bar is 22pt tall) |
| Author at | 64–72px square (4×) |
| Format | PNG with alpha |
| Colour | **Pure black + alpha only** |

A **template image**. The system discards the colour and tints the alpha
channel, so black + alpha renders white on a dark menu bar and black on a light
one, automatically and correctly. Drawing it white makes it invisible in light
mode.

Flagging it as a template, by framework:

| Framework | How |
|---|---|
| AppKit | `image.isTemplate = true`, or name the file `fooTemplate.png` |
| Electron | Filename ending in `Template` (e.g. `trayTemplate.png`) is auto-detected; or `nativeImage.setTemplateImage(true)` |
| Tauri v2 | `TrayIconBuilder::new().icon_as_template(true)` |

Detail punched out with a mask stays transparent and tints correctly. Detail
painted in a background colour does not.

## iOS / iPadOS

| Property | Value |
|---|---|
| Source | 1024 × 1024 |
| Corners | **Square** — the system masks them |
| Alpha | **None** — must be fully opaque |
| Format | PNG |

Pre-rounding an iOS icon gets it rounded twice, which is visible and looks
broken. Xcode's asset catalog generates the rest from the single 1024.

## Android — adaptive icons

| Layer | Size | Notes |
|---|---|---|
| Full layer | 108 × 108 dp | Foreground and background are separate layers |
| Visible viewport | 72 × 72 dp | Centered |
| Safe zone | 66 dp diameter circle | Centered — nothing important outside it |

The launcher masks to whatever shape the OEM chose (circle, squircle, teardrop),
and animates the layers in parallax. Anything outside the safe circle can be
cropped on some devices and not others.

Legacy fallback: `mipmap-*` PNGs at 48 / 72 / 96 / 144 / 192 for mdpi through
xxxhdpi.

## Windows

**App icon** — one `.ico` containing 16, 24, 32, 48, 64, 128, 256. The 256 entry
should be PNG-compressed inside the container to keep file size sane.

**Taskbar / system tray** — 16 @ 100% DPI, and additionally 20 (125%), 24
(150%), 32 (200%). No template-image concept: the tray needs the **colour**
icon, since a pure black glyph vanishes into a dark taskbar.

**MSIX / Store tiles** — `Square44x44Logo`, `Square71x71Logo`,
`Square107x107Logo`, `Square142x142Logo`, `Square150x150Logo`,
`Square284x284Logo`, `Square310x310Logo`, `StoreLogo` (50×50). Tauri generates
this whole set.

## Linux

**Desktop icons** — the hicolor theme, sizes 16, 22, 24, 32, 48, 64, 128, 256:

```
/usr/share/icons/hicolor/<size>x<size>/apps/<name>.png
/usr/share/icons/hicolor/scalable/apps/<name>.svg
```

Shipping the scalable SVG alongside is worth it — modern desktops prefer it.

**Tray** (StatusNotifierItem / AppIndicator) — roughly 22–24px, full colour. No
template images; use the colour icon.

## Web — the modern favicon set

The minimum set that covers every current browser and platform:

| File | Size | Purpose |
|---|---|---|
| `favicon.ico` | 16 + 32 + 48 multi-res | Legacy browsers, and the bookmark bar |
| `favicon.svg` | vector | Preferred by every modern browser; scales free |
| `apple-touch-icon.png` | 180 × 180 | iOS home screen — **must be opaque** |
| `icon-192.png` | 192 × 192 | Web app manifest |
| `icon-512.png` | 512 × 512 | Manifest, splash screens |
| `icon-maskable-512.png` | 512 × 512 | Manifest, `purpose: "maskable"` |

```html
<link rel="icon" href="/favicon.svg" type="image/svg+xml">
<link rel="alternate icon" href="/favicon.ico" sizes="16x16 32x32 48x48">
<link rel="apple-touch-icon" href="/apple-touch-icon.png">
<link rel="manifest" href="/site.webmanifest">
```

```json
{
  "icons": [
    { "src": "/icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "/icon-512.png", "sizes": "512x512", "type": "image/png" },
    { "src": "/icon-maskable-512.png", "sizes": "512x512",
      "type": "image/png", "purpose": "maskable" }
  ]
}
```

Three things that bite:

- **`apple-touch-icon` must be opaque.** iOS composites transparency onto black,
  so a transparent-cornered icon gets black corners on the home screen.
- **Maskable safe zone** is a centered circle of diameter **80%** of the image
  (radius 40%). Everything outside it is croppable. A maskable icon therefore
  needs a full-bleed background and a *smaller* focal element than the standard
  icon — it is a different layout, not the same file relabelled.
- **The 16px favicon is the real design constraint.** It is where the icon
  actually lives most of the time. Design for it first, not last.

### Theme-reactive SVG favicon

An SVG favicon can respond to the browser's colour scheme, which nothing else in
the set can do:

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32">
  <style>
    .fg { fill: #14110f; }
    @media (prefers-color-scheme: dark) { .fg { fill: #f5f0eb; } }
  </style>
  <path class="fg" d="…" />
</svg>
```

Keep it to a handful of paths — this one renders at 16px constantly.

---

## Framework generators

**Tauri v2** — `tauri icon path/to/icon-1024.png` fans out `.icns`, `.ico`, and
every PNG into `src-tauri/icons/`. It generates iOS and Android sets too; delete
them for a desktop-only app. The tray glyph is *not* generated — render that
separately from its own SVG.

**Electron** — needs `.icns` (mac), `.ico` (win), and PNGs (linux) supplied
directly; `electron-builder` reads them from a `build/` directory. Tray icons
follow the `Template` filename convention on macOS.

**PWA / web** — no standard generator worth trusting; render each size from the
SVG with the pipeline in `pipeline.md`.
