# Godot 4 Import Notes

## Recommended setup
1. Open the Godot project and copy this repository's `assets/` directory into the project.
2. Import SVG textures at the render resolution needed by the game. Keep the source SVGs in version control.
3. For crisp pixel-art-like edges, set texture filtering to **Nearest** on the relevant CanvasItem/texture import settings.
4. Use `AtlasTexture` or `Sprite2D` with `region_enabled` and `region_rect` for individual atlas cells.
5. For the isometric world, configure a TileSet with a 64×32 isometric tile size and verify the engine's tile shape/offset convention before painting maps.

## Asset sheet sizes
| File | Canvas | Suggested cell |
|---|---:|---:|
| `tiles/terrain_atlas.svg` | 384×192 | 64×32 |
| `buildings/building_atlas.svg` | 384×256 | 128×128 |
| `nature/nature_atlas.svg` | 256×192 | 64×64 |
| `props/props_atlas.svg` | 320×192 | 64×64 |
| `characters/resident_atlas.svg` | 256×256 | 64×64 |
| `vehicles/vehicle_atlas.svg` | 256×128 | 128×64 |
| `ui/ui_atlas.svg` | 256×128 | 64×64 |

## Important limitations
- These are a first cohesive art pass, not a finished production art library.
- Resident cells are static directional poses; walk cycles, emotes, layered outfits, shadows, and rig/animation resources still need to be authored.
- Building art is decorative illustration. A game scene still needs collision polygons, footprints, occlusion rules, interaction points, and tile/map placement data.
- Do not assume all cells are interchangeable gameplay tiles; some sheets are prop atlases rather than terrain TileSets.
