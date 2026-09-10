# Amber Console composite recipes

Create a repeatable theme showcase with `theme_composite_tool`. This example uses the six screenshots in the Amber Console workspace and Jesse Palmer's IMAX console photograph. Choose clock positions for the screenshots, then choose whether the background fills the canvas or remains completely visible in its center.

The [editable recipe](https://framemoowork.local:8443/knowledge?item=dGhlbWVzL29tYXJjaHktYW1iZXItY29uc29sZS10aGVtZS9zaG93Y2FzZS5qc29u&view=document) lives beside the theme README as `showcase.json`. Edit it in vi or Revelator, then render it from the theme directory:

```bash
cd "$HOME/dil/themes/omarchy-amber-console-theme"
theme_composite_tool render showcase.json --output screenshots/showcase-center.jpg --replace --json
theme_composite_tool render showcase.json --output screenshots/showcase-fill.jpg --background-mode fill --replace --json
```

## Keep the complete background in the center

`center` scales the whole background into the center column, preserving its proportions. This is the stronger presentation for a console photograph whose details matter.

![Amber Console with six screenshots surrounding the complete IMAX console photograph](screenshots/showcase-center.jpg)

## Fill the entire canvas

`fill` scales the background to cover the whole canvas; excess edges may be cropped. Screenshots and text backdrops sit over it. This mode also works with scenic or abstract wallpapers.

![Amber Console with six screenshots over an IMAX photograph filling the canvas](screenshots/showcase-fill.jpg)

These are local worked examples. The theme's published preview remains a separate `preview.jpg` asset, managed through the existing theme publication workflow.

## Place each screenshot

| Left column | Center column | Right column |
| --- | --- | --- |
| 10 — top-left | 12 — optional top panel | 2 — top-right |
| 9 — left | background | 3 — right |
| 8 — bottom-left | 6 — optional bottom panel | 4 — bottom-right |

Named positions such as `top-left` work too. Clock hours 11, 1, 7 and 5 are aliases for their neighboring diagonal positions. Two images cannot occupy the same position.

The example uses fastfetch at 10, Neovim at 9, Yazi at 8, btop at 2, Files at 3 and Sigye at 4. Remove the two middle-side entries for a four-corner layout with larger panels.

For another theme, create a recipe with your own files:

```bash
theme_composite_tool init showcase.json --title 'My Theme' \
  --background backgrounds/wallpaper.jpg --background-mode center \
  --panel 10=screenshots/terminal.png --panel 2=screenshots/btop.png \
  --panel 8=screenshots/editor.png --panel 4=screenshots/files.png
```

Paths in the saved recipe are relative to that recipe. Quote a complete `position=path` argument when the path contains spaces.

## Adjust the recipe

- `background.mode`: `center` or `fill`.
- `panels[].fit`: `contain` preserves the full screenshot; `cover` crops it to fill its assigned panel. The default is `contain`.
- `canvas`: width, height and background color; default 3200 × 1800.
- `style`: accent, text color, local font family or font file, margin, gap and border.
- `title`, `subtitle`, `footer`: editable banner text. The renderer fits these to their allocated areas.

Use `theme_composite_tool plan showcase.json --json` to inspect the layout and source hashes. Use `render ... --dry-run --json` for preflight without writing an image.

Each render also writes a sidecar such as `showcase-center.jpg.render.json`, recording the recipe, exact source hashes, resolved font, renderer version, geometry, authored text and output hash. Repeat rendering produces identical image bytes when the recipe, inputs and recorded toolchain are unchanged. Different existing outputs require `--replace`; the tool rejects overwriting a source image, recipe or font.

Version 1 consumes local PNG, JPEG and WebP images and produces PNG or JPEG. Knowledge intake, OCR and collections stay with the image-handling program; a future picker can supply those governed assets to the same recipe interface.

For the theme's attribution, credits, license and installation, see [README.md](README.md).
