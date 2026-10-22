# Asset Pack — Virtual City

Original, placeholder-free **source vector assets** for the Virtual City game. These are original designs made for this repository, not extracted from Ragnarok Online, Thaloria, or the reference images.

## Art direction
- Isometric pixel-art-inspired city, warm cozy palette, dark 1–2 px outlines, compact shapes.
- SVG source files use crisp edges and a 2:1 isometric tile convention (64×32).
- Intended for Godot 4 import as textures; SVGs can be rasterized at the target project scale.
- Character motion is autonomous; these sprites are visual assets only and do not implement simulation logic.

## Inventory
- `tiles/terrain_atlas.svg`: grass, path, road, water, paving and road markings.
- `buildings/building_atlas.svg`: house, apartment, shop and civic building silhouettes.
- `nature/nature_atlas.svg`: trees, shrubs and flower patch.
- `props/props_atlas.svg`: bench, lamp, sign, planter and fountain.
- `characters/resident_atlas.svg`: original chibi resident directions and palette variants.
- `vehicles/vehicle_atlas.svg`: compact car and city van.
- `ui/ui_atlas.svg`: chat bubble, location marker, interaction icons and panel ornaments.

## Integration notes
These are atlas/source sheets, not a complete playable project. Each SVG includes a transparent background and labelled cells where applicable. In Godot, import as texture, disable filtering for pixel-crisp rendering, and use AtlasTexture/region rectangles or split the sheet into individual sprites. Review each file's viewBox and cell dimensions before assigning regions.

## License
Repository-authored assets. See `LICENSE-ASSETS.md` for usage terms.
