# Palette & usage

- Deep outline: `#34483E`
- Leaf green: `#5F8A65`
- Light leaf: `#8DB47B`
- Cream plaster: `#E8D9B8`
- Warm clay: `#C77E63`
- Muted teal: `#78AFA8`
- Golden accent: `#E3B85D`
- Wood: `#89664B`

## Environment atlas
`environment_atlas.svg` is 512×512, 4 columns × 4 rows, cell size 128×128. Cells are ordered left-to-right, top-to-bottom: round tree, young tree, flower planter, shrub planter; streetlamp, wooden bench, cafe table, public fountain; bus stop, notice board, mailbox, trash bin; bicycle rack, bollards, vending kiosk, directional sign.

## Resident variants
`resident_variants.svg` is 384×384, 4 columns × 4 rows, cell size 96×96. Each cell contains one full-body front-facing resident sprite, with transparent background and a subtle ground shadow. These are static poses, not animated walk cycles. To animate, create separate walk-cycle frames per direction rather than stretching or reusing the static pose.

SVGs can be previewed directly in a browser and converted to PNG/WebP with a rasterizer as part of an asset pipeline. Preserve SVG source files as the editable source of truth.
