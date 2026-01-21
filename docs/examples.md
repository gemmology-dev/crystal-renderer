# Examples

## Basic SVG Generation

### From CDL String

```python
from crystal_renderer import generate_cdl_svg

# Simple octahedron
generate_cdl_svg("cubic[m3m]:{111}", "octahedron.svg")

# Diamond-like truncated octahedron
generate_cdl_svg(
    "cubic[m3m]:{111}@1.0 + {100}@1.3",
    "diamond.svg",
    show_axes=True
)

# Quartz prism with rhombohedron
generate_cdl_svg(
    "trigonal[-3m]:{10-10}@1.0 + {10-11}@0.8",
    "quartz.svg"
)
```

### From Geometry

```python
from crystal_geometry import cdl_string_to_geometry
from crystal_renderer import generate_geometry_svg

# Generate geometry
geom = cdl_string_to_geometry("cubic[m3m]:{111}@1.0 + {100}@1.3")

# Render with custom colors
generate_geometry_svg(
    geom.vertices,
    geom.faces,
    "custom.svg",
    face_color='#81D4FA',
    edge_color='#0277BD'
)
```

## Customizing Appearance

### View Angles

```python
from crystal_renderer import generate_cdl_svg

cdl = "cubic[m3m]:{111}"

# Different viewing angles
generate_cdl_svg(cdl, "view1.svg", elev=30, azim=-45)   # Standard view
generate_cdl_svg(cdl, "view2.svg", elev=0, azim=0)      # Front view
generate_cdl_svg(cdl, "view3.svg", elev=90, azim=0)     # Top view
generate_cdl_svg(cdl, "view4.svg", elev=15, azim=-30)   # Low angle
```

### Display Options

```python
from crystal_renderer import generate_cdl_svg

cdl = "cubic[m3m]:{111}@1.0 + {100}@1.3"

# With axes and grid
generate_cdl_svg(
    cdl, "with_axes.svg",
    show_axes=True,
    show_grid=True
)

# Color by form
generate_cdl_svg(
    cdl, "colored.svg",
    color_by_form=True
)

# With face labels
generate_cdl_svg(
    cdl, "labeled.svg",
    face_labels=True
)
```

### Custom Colors

```python
from crystal_renderer import generate_geometry_svg
from crystal_geometry import cdl_string_to_geometry

geom = cdl_string_to_geometry("cubic[m3m]:{111}")

# Custom face and edge colors
generate_geometry_svg(
    geom.vertices,
    geom.faces,
    "ruby.svg",
    face_color='#E91E63',    # Ruby red
    edge_color='#880E4F'     # Dark red edges
)

# Emerald green
generate_geometry_svg(
    geom.vertices,
    geom.faces,
    "emerald.svg",
    face_color='#4CAF50',
    edge_color='#1B5E20'
)
```

## 3D Export

### STL for 3D Printing

```python
from crystal_geometry import cdl_string_to_geometry
from crystal_renderer import export_stl

# Generate geometry
geom = cdl_string_to_geometry("cubic[m3m]:{111}@1.0 + {100}@1.3")

# Export binary STL (smaller file)
export_stl(geom.vertices, geom.faces, "crystal.stl", binary=True)

# Export ASCII STL (human-readable)
export_stl(geom.vertices, geom.faces, "crystal_ascii.stl", binary=False)
```

### glTF for Web/AR

```python
from crystal_geometry import cdl_string_to_geometry
from crystal_renderer import export_gltf

geom = cdl_string_to_geometry("cubic[m3m]:{111}")

# Basic export
export_gltf(geom.vertices, geom.faces, "model.gltf")

# With custom color and name
export_gltf(
    geom.vertices,
    geom.faces,
    "diamond.gltf",
    color=(0.5, 0.7, 0.9, 0.8),  # Light blue, semi-transparent
    name="diamond_crystal"
)
```

## Raster Output

### PNG Export

```python
from crystal_renderer import generate_cdl_svg, convert_svg_to_raster

# First generate SVG
generate_cdl_svg("cubic[m3m]:{111}", "crystal.svg")

# Convert to PNG
convert_svg_to_raster("crystal.svg", "crystal.png", scale=2.0)
```

### Direct Raster Generation

```python
from crystal_renderer import generate_with_format, generate_cdl_svg

# Generate PNG directly
generate_with_format(
    generator_func=generate_cdl_svg,
    output_path="crystal.png",
    output_format="png",
    cdl_string="cubic[m3m]:{111}",
    show_axes=True
)

# Generate JPG
generate_with_format(
    generator_func=generate_cdl_svg,
    output_path="crystal.jpg",
    output_format="jpg",
    cdl_string="cubic[m3m]:{111}"
)
```

## Info Panels

### Adding Property Panels

```python
from crystal_renderer import generate_cdl_svg

# With FGA-style info panel
properties = {
    'name': 'Diamond',
    'chemistry': 'C',
    'hardness': '10',
    'ri': '2.417',
    'sg': '3.52',
    'dispersion': '0.044'
}

generate_cdl_svg(
    "cubic[m3m]:{111}@1.0 + {110}@0.2",
    "diamond_info.svg",
    info_panel=properties,
    show_axes=True
)
```

### Integration with mineral-database

```python
from mineral_database import get_mineral, get_info_properties
from crystal_renderer import generate_cdl_svg

# Get mineral data
mineral = get_mineral('ruby')
props = get_info_properties('ruby', 'fga')

# Render with info panel
generate_cdl_svg(
    mineral.cdl,
    "ruby_info.svg",
    info_panel=props
)
```

## Working with Colors

### Color Schemes

```python
from crystal_renderer import (
    AXIS_COLOURS,
    HABIT_COLOURS,
    FORM_COLORS,
    get_element_colour
)

# Axis colors
print(f"a-axis: {AXIS_COLOURS['a']}")  # Red
print(f"b-axis: {AXIS_COLOURS['b']}")  # Green
print(f"c-axis: {AXIS_COLOURS['c']}")  # Blue

# System colors
print(f"Cubic: {HABIT_COLOURS['cubic']}")
print(f"Hexagonal: {HABIT_COLOURS['hexagonal']}")

# Element colors (for atomic structures)
print(f"Carbon: {get_element_colour('C')}")
print(f"Silicon: {get_element_colour('Si')}")
```

### Multi-Form Coloring

```python
from crystal_renderer import generate_cdl_svg

# Color each form differently
generate_cdl_svg(
    "cubic[m3m]:{111}@1.0 + {100}@1.3 + {110}@0.5",
    "multi_form.svg",
    color_by_form=True  # Each form gets distinct color
)
```

## Batch Processing

### Rendering Multiple Minerals

```python
from mineral_database import list_presets, get_preset
from crystal_renderer import generate_cdl_svg

# Render all cubic minerals
for preset_id in list_presets('cubic')[:10]:
    preset = get_preset(preset_id)
    generate_cdl_svg(
        preset['cdl'],
        f"output/{preset_id}.svg",
        show_axes=True
    )
    print(f"Rendered {preset_id}")
```

### Gallery Generation

```python
from mineral_database import list_presets, get_preset, get_info_properties
from crystal_renderer import generate_cdl_svg
import os

# Create output directory
os.makedirs("gallery", exist_ok=True)

# Generate gallery
for preset_id in list_presets()[:20]:
    preset = get_preset(preset_id)
    props = get_info_properties(preset_id, 'compact')

    generate_cdl_svg(
        preset['cdl'],
        f"gallery/{preset_id}.svg",
        info_panel=props,
        elev=25,
        azim=-40
    )
```

## Full Pipeline Example

```python
from mineral_database import get_mineral
from cdl_parser import parse_cdl
from crystal_geometry import cdl_to_geometry
from crystal_renderer import (
    generate_geometry_svg,
    export_stl,
    export_gltf
)

# Get mineral data
mineral = get_mineral('garnet')
print(f"Processing: {mineral.name}")

# Parse CDL
desc = parse_cdl(mineral.cdl)
print(f"System: {desc.system}, Forms: {len(desc.forms)}")

# Generate geometry
geom = cdl_to_geometry(desc)
print(f"Vertices: {len(geom.vertices)}, Faces: {len(geom.faces)}")

# Export to multiple formats
generate_geometry_svg(
    geom.vertices,
    geom.faces,
    f"{mineral.id}.svg",
    face_color='#E57373'  # Garnet red
)

export_stl(
    geom.vertices,
    geom.faces,
    f"{mineral.id}.stl"
)

export_gltf(
    geom.vertices,
    geom.faces,
    f"{mineral.id}.gltf",
    color=(0.9, 0.3, 0.3, 1.0),
    name=mineral.name
)

print(f"Exported SVG, STL, and glTF for {mineral.name}")
```
