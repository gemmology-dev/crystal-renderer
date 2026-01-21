# Crystal Renderer

**SVG and 3D Visualization** - Renders crystal structures from CDL notation with export to multiple formats including SVG, STL, and glTF.

Part of the [Gemmology Project](https://gemmology.dev).

## Overview

Crystal Renderer provides comprehensive visualization capabilities:

- **CDL Visualization**: Generate SVG images from Crystal Description Language notation
- **Multi-Format Export**: SVG, PNG, JPG, STL, glTF
- **Customizable Rendering**: Face colors, axes, grid, labels
- **Info Panels**: FGA-style property panels on visualizations
- **3D Projection**: Configurable elevation and azimuth angles
- **Color Schemes**: System-based and form-based coloring

## Installation

```bash
pip install gemmology-crystal-renderer
```

For raster image export (PNG, JPG):

```bash
pip install gemmology-crystal-renderer[raster]
```

### Requirements

- Python >= 3.10
- numpy >= 1.20.0
- matplotlib >= 3.5.0
- gemmology-cdl-parser >= 1.0.0
- gemmology-crystal-geometry >= 1.0.0

### Optional Dependencies

- `cairosvg` - SVG to raster conversion
- `Pillow` - Image processing
- `ase` - Atomic structure visualization

## Quick Start

```python
from crystal_renderer import generate_cdl_svg

# Generate SVG from CDL notation
generate_cdl_svg("cubic[m3m]:{111}@1.0 + {100}@1.3", "crystal.svg")

# Export to 3D formats
from crystal_geometry import create_octahedron
from crystal_renderer import export_stl, export_gltf

geom = create_octahedron()
export_stl(geom.vertices, geom.faces, "octahedron.stl")
export_gltf(geom.vertices, geom.faces, "octahedron.gltf")
```

## Output Formats

| Format | Extension | Description |
|--------|-----------|-------------|
| SVG | `.svg` | Vector graphics, scalable |
| PNG | `.png` | Raster with transparency |
| JPG | `.jpg` | Compressed raster |
| STL | `.stl` | 3D printing format |
| glTF | `.gltf` | Web/AR 3D format |

## Related Packages

- [cdl-parser](https://cdl-parser.gemmology.dev) - Crystal Description Language parser
- [crystal-geometry](https://crystal-geometry.gemmology.dev) - 3D geometry engine
- [mineral-database](https://mineral-database.gemmology.dev) - Mineral preset database

## License

MIT License - see [LICENSE](https://github.com/gemmology-dev/crystal-renderer/blob/main/LICENSE) for details.
