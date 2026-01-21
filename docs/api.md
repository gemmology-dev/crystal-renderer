# API Reference

## High-Level Visualization

### generate_cdl_svg

Generate SVG visualization from a CDL string.

```python
from crystal_renderer import generate_cdl_svg

generate_cdl_svg(
    cdl_string="cubic[m3m]:{111}@1.0",
    output_path="crystal.svg",
    show_axes=True,
    elev=30,
    azim=-45,
    color_by_form=False,
    show_grid=True,
    face_labels=False
)
```

::: crystal_renderer.generate_cdl_svg

### generate_geometry_svg

Generate SVG from raw geometry data.

```python
from crystal_renderer import generate_geometry_svg

generate_geometry_svg(
    vertices=geom.vertices,
    faces=geom.faces,
    output_path="geometry.svg",
    face_color='#81D4FA',
    edge_color='#0277BD'
)
```

::: crystal_renderer.generate_geometry_svg

## 3D Export

### export_stl

Export geometry to STL format for 3D printing.

```python
from crystal_renderer import export_stl

export_stl(vertices, faces, "model.stl", binary=True)
```

::: crystal_renderer.export_stl

### export_gltf

Export geometry to glTF format for web/AR.

```python
from crystal_renderer import export_gltf

export_gltf(
    vertices, faces, "model.gltf",
    color=(0.5, 0.7, 0.9, 0.8),  # RGBA
    name="crystal"
)
```

::: crystal_renderer.export_gltf

## Format Conversion

### convert_svg_to_raster

Convert existing SVG to raster format.

```python
from crystal_renderer import convert_svg_to_raster

convert_svg_to_raster("input.svg", "output.png", scale=2.0)
```

::: crystal_renderer.convert_svg_to_raster

### generate_with_format

Generate visualization directly to any supported format.

```python
from crystal_renderer import generate_with_format, generate_cdl_svg

generate_with_format(
    generator_func=generate_cdl_svg,
    output_path="crystal.png",
    output_format="png",
    cdl_string="cubic[m3m]:{111}"
)
```

::: crystal_renderer.generate_with_format

## Info Panels

### render_info_panel

Render an info panel on a matplotlib axes.

```python
from crystal_renderer import render_info_panel

properties = {
    'name': 'Ruby',
    'chemistry': 'Al2O3',
    'hardness': '9',
    'ri': '1.762-1.770'
}
render_info_panel(ax, properties, position='top-right', style='compact')
```

::: crystal_renderer.render_info_panel

### create_fga_info_panel

Create FGA-style property panel from mineral data.

```python
from crystal_renderer import create_fga_info_panel

fga_props = create_fga_info_panel(mineral_data)
```

::: crystal_renderer.create_fga_info_panel

## Color Constants

### AXIS_COLOURS

Colors for crystallographic axes (a, b, c).

```python
from crystal_renderer import AXIS_COLOURS

print(AXIS_COLOURS['a'])  # Red
print(AXIS_COLOURS['b'])  # Green
print(AXIS_COLOURS['c'])  # Blue
```

### ELEMENT_COLOURS

Colors for chemical elements.

```python
from crystal_renderer import ELEMENT_COLOURS, get_element_colour

color = get_element_colour('Si')  # '#F0C8A0'
color = get_element_colour('O')   # '#FF0000'
```

::: crystal_renderer.get_element_colour

### HABIT_COLOURS

Colors for crystal systems/habits.

```python
from crystal_renderer import HABIT_COLOURS

print(HABIT_COLOURS['cubic'])     # Color for cubic system
print(HABIT_COLOURS['trigonal'])  # Color for trigonal system
```

### FORM_COLORS

Colors for multi-form rendering.

```python
from crystal_renderer import FORM_COLORS

# Colors for distinguishing different crystal forms
# e.g., {111} in one color, {100} in another
```

## Projection Utilities

### calculate_view_direction

Calculate the view direction vector from elevation and azimuth.

```python
from crystal_renderer import calculate_view_direction

view = calculate_view_direction(elev=30, azim=-45)
```

::: crystal_renderer.calculate_view_direction

### calculate_axis_origin

Calculate the origin point for axis rendering.

```python
from crystal_renderer import calculate_axis_origin

origin = calculate_axis_origin(vertices, padding=0.1)
```

::: crystal_renderer.calculate_axis_origin

### calculate_vertex_visibility

Calculate which vertices are front-facing.

```python
from crystal_renderer import calculate_vertex_visibility

visibility = calculate_vertex_visibility(vertices, faces, elev=30, azim=-45)
```

::: crystal_renderer.calculate_vertex_visibility

### is_face_visible

Check if a face is visible from the current view direction.

```python
from crystal_renderer import is_face_visible

visible = is_face_visible(face_normal, view_direction)
```

::: crystal_renderer.is_face_visible
