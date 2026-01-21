# Atomic Structure Visualization

Crystal-renderer provides functions for visualizing atomic crystal structures, including atom positions, bonds, coordination polyhedra, and unit cells. These features integrate with the Atomic Simulation Environment (ASE) for atomic structure handling.

## Requirements

Atomic visualization features require optional dependencies:

```bash
pip install gemmology-crystal-renderer[atomic]
# Or install ASE directly:
pip install ase
```

---

## Element Colors and Radii

### Built-in Color Scheme

```python
from crystal_renderer import ELEMENT_COLOURS

# Predefined colors based on Jmol scheme
print(ELEMENT_COLOURS)
# {'C': '#909090', 'O': '#FF0D0D', 'Al': '#BFA6A6', ...}
```

| Element | Color | Hex |
|---------|-------|-----|
| C (Carbon) | Grey | `#909090` |
| O (Oxygen) | Red | `#FF0D0D` |
| Si (Silicon) | Beige | `#F0C8A0` |
| Al (Aluminium) | Pink-grey | `#BFA6A6` |
| Mg (Magnesium) | Bright green | `#8AFF00` |
| Fe (Iron) | Orange | `#E06633` |
| Cr (Chromium) | Blue-grey | `#8A99C7` |
| Zr (Zirconium) | Cyan | `#94E0E0` |
| F (Fluorine) | Green | `#90E050` |
| Ca (Calcium) | Green | `#3DFF00` |

### Getting Element Properties

```python
from crystal_renderer import get_element_colour, get_element_radius

# Get color for any element (falls back to Jmol colors via ASE)
color = get_element_colour('Si')  # '#F0C8A0'
color = get_element_colour('Au')  # Uses ASE Jmol colors

# Get display radius (based on covalent radii)
radius = get_element_radius('Si')  # ~0.55 Å
radius = get_element_radius('O')   # ~0.32 Å
```

---

## Visualizing Atomic Structures

### Basic Atom Rendering

```python
import matplotlib.pyplot as plt
from ase import Atoms
from ase.build import bulk
from crystal_renderer import (
    draw_atoms,
    draw_bonds,
    draw_atom_labels,
    set_axes_equal,
    hide_axes_and_grid,
)

# Create a structure (diamond cubic silicon)
atoms = bulk('Si', 'diamond', a=5.43)

# Set up 3D plot
fig = plt.figure(figsize=(10, 10))
ax = fig.add_subplot(111, projection='3d')

# Draw atoms, bonds, and labels
draw_atoms(ax, atoms)
draw_bonds(ax, atoms, cutoff=2.5)
draw_atom_labels(ax, atoms)

# Clean up display
set_axes_equal(ax)
hide_axes_and_grid(ax)

plt.savefig('silicon_structure.svg')
plt.show()
```

### Drawing Unit Cell

```python
import matplotlib.pyplot as plt
from ase.build import bulk
from crystal_renderer import (
    draw_atoms,
    draw_bonds,
    draw_unit_cell_box,
    set_axes_equal,
    hide_axes_and_grid,
)

# Create corundum structure
from ase.build import bulk
atoms = bulk('Al2O3', 'corundum', a=4.76, c=13.0)

# Get cell parameters
cellpar = atoms.cell.cellpar().tolist()  # [a, b, c, alpha, beta, gamma]

# Set up plot
fig = plt.figure(figsize=(10, 10))
ax = fig.add_subplot(111, projection='3d')
ax.view_init(elev=20, azim=-60)

# Draw unit cell box with axes
draw_unit_cell_box(ax, cellpar, show_axes=True)

# Draw atoms and bonds
draw_atoms(ax, atoms)
draw_bonds(ax, atoms, cutoff=2.0)

set_axes_equal(ax)
plt.savefig('corundum_unit_cell.svg')
```

---

## Coordination Polyhedra

Visualize the coordination environment around specific atoms:

```python
import matplotlib.pyplot as plt
from ase.build import bulk
from crystal_renderer import (
    draw_atoms,
    draw_coordination_polyhedra,
    set_axes_equal,
    hide_axes_and_grid,
)

# Create a spinel structure (MgAl2O4)
from ase.spacegroup import crystal

spinel = crystal(
    symbols=['Mg', 'Al', 'O'],
    basis=[(0.125, 0.125, 0.125), (0.5, 0.5, 0.5), (0.262, 0.262, 0.262)],
    spacegroup=227,  # Fd-3m
    cellpar=[8.08, 8.08, 8.08, 90, 90, 90],
)

# Set up plot
fig = plt.figure(figsize=(12, 12))
ax = fig.add_subplot(111, projection='3d')

# Draw atoms
draw_atoms(ax, spinel)

# Draw octahedral coordination around Al (Al-O polyhedra)
draw_coordination_polyhedra(ax, spinel, 'Al', 'O', cutoff=2.5)

# Draw tetrahedral coordination around Mg (Mg-O polyhedra)
draw_coordination_polyhedra(ax, spinel, 'Mg', 'O', cutoff=2.3)

set_axes_equal(ax)
hide_axes_and_grid(ax)
plt.savefig('spinel_coordination.svg')
```

---

## Legend and Labels

### Element Legend

```python
from crystal_renderer import draw_legend

# After drawing atoms
elements_used = set(atoms.get_chemical_symbols())
draw_legend(ax, elements_used)
```

### Custom Atom Labels

```python
from crystal_renderer import draw_atom_labels

# Draw labels with custom offset
draw_atom_labels(ax, atoms, offset=0.5)  # Further from atoms
draw_atom_labels(ax, atoms, offset=0.2)  # Closer to atoms
```

---

## Complete Example: Corundum (Ruby/Sapphire)

```python
"""Visualize corundum (Al2O3) crystal structure."""
import matplotlib.pyplot as plt
from ase.spacegroup import crystal
from crystal_renderer import (
    draw_atoms,
    draw_bonds,
    draw_atom_labels,
    draw_coordination_polyhedra,
    draw_unit_cell_box,
    draw_legend,
    set_axes_equal,
    hide_axes_and_grid,
    ELEMENT_COLOURS,
)

# Build corundum structure (trigonal, R-3c)
corundum = crystal(
    symbols=['Al', 'O'],
    basis=[
        (0.0, 0.0, 0.3523),   # Al position
        (0.3064, 0.0, 0.25),  # O position
    ],
    spacegroup=167,  # R-3c
    cellpar=[4.76, 4.76, 12.99, 90, 90, 120],
)

# Create visualization
fig = plt.figure(figsize=(12, 12))
ax = fig.add_subplot(111, projection='3d')
ax.view_init(elev=25, azim=-50)

# Get cell parameters for unit cell
cellpar = corundum.cell.cellpar().tolist()

# Draw all components
draw_unit_cell_box(ax, cellpar, show_axes=True)
draw_atoms(ax, corundum)
draw_bonds(ax, corundum, cutoff=2.1)
draw_atom_labels(ax, corundum, offset=0.4)
draw_coordination_polyhedra(ax, corundum, 'Al', 'O', cutoff=2.1)

# Add legend
elements = set(corundum.get_chemical_symbols())
draw_legend(ax, elements)

# Clean up
set_axes_equal(ax)
ax.set_title('Corundum (Al₂O₃) - Ruby/Sapphire Structure', fontsize=14)

plt.tight_layout()
plt.savefig('corundum_structure.svg', dpi=150, bbox_inches='tight')
plt.savefig('corundum_structure.png', dpi=150, bbox_inches='tight')
plt.show()
```

---

## Advanced: Custom Color Schemes

### Override Element Colors

```python
from crystal_renderer import ELEMENT_COLOURS

# Modify for specific visualization
ELEMENT_COLOURS['Al'] = '#FF6B6B'  # Make aluminium red (for Cr substitution in ruby)
ELEMENT_COLOURS['O'] = '#4ECDC4'   # Teal oxygen

# Now all visualizations use these colors
```

### Per-Atom Coloring

For more control, access atoms directly:

```python
import numpy as np
from crystal_renderer import get_element_colour

def draw_atoms_custom(ax, atoms, color_map=None):
    """Draw atoms with custom per-atom colors."""
    positions = atoms.get_positions()
    symbols = atoms.get_chemical_symbols()

    for i, (pos, sym) in enumerate(zip(positions, symbols)):
        if color_map and i in color_map:
            color = color_map[i]
        else:
            color = get_element_colour(sym)

        # Draw sphere
        u = np.linspace(0, 2 * np.pi, 20)
        v = np.linspace(0, np.pi, 10)
        r = 0.5  # radius
        x = r * np.outer(np.cos(u), np.sin(v)) + pos[0]
        y = r * np.outer(np.sin(u), np.sin(v)) + pos[1]
        z = r * np.outer(np.ones(np.size(u)), np.cos(v)) + pos[2]
        ax.plot_surface(x, y, z, color=color, alpha=0.9)

# Highlight specific atoms
custom_colors = {0: '#FF0000', 5: '#00FF00'}  # Atoms 0 and 5 highlighted
draw_atoms_custom(ax, atoms, color_map=custom_colors)
```

---

## Bond Visualization Options

### Adjusting Bond Cutoff

```python
from crystal_renderer import draw_bonds

# Short bonds only (ionic crystals)
draw_bonds(ax, atoms, cutoff=2.0)

# Longer bonds (molecular crystals)
draw_bonds(ax, atoms, cutoff=3.5)

# Find optimal cutoff automatically
from ase.neighborlist import natural_cutoffs
cutoffs = natural_cutoffs(atoms)
max_cutoff = max(cutoffs) * 2
draw_bonds(ax, atoms, cutoff=max_cutoff)
```

### Bicolor Bonds

The default `draw_bonds` function draws bonds with two colors, split at the midpoint:

```
[Atom A color] -------|------- [Atom B color]
```

This clearly shows which atoms are bonded while maintaining element identification.

---

## Viewing Angles

Set viewing angles to highlight specific crystal features:

```python
# Standard isometric view
ax.view_init(elev=30, azim=-45)

# View down c-axis
ax.view_init(elev=90, azim=0)

# View down a-axis
ax.view_init(elev=0, azim=0)

# View down b-axis
ax.view_init(elev=0, azim=90)

# Hexagonal systems - view down [001]
ax.view_init(elev=90, azim=-30)  # Align with hexagonal symmetry
```

---

## Performance Tips

### Large Structures

For structures with many atoms:

```python
# Reduce sphere resolution for faster rendering
def draw_atoms_fast(ax, atoms, resolution=10):
    """Fast atom drawing with lower resolution spheres."""
    positions = atoms.get_positions()
    symbols = atoms.get_chemical_symbols()

    u = np.linspace(0, 2 * np.pi, resolution)
    v = np.linspace(0, np.pi, resolution // 2)

    for pos, sym in zip(positions, symbols):
        color = get_element_colour(sym)
        radius = get_element_radius(sym)

        x = radius * np.outer(np.cos(u), np.sin(v)) + pos[0]
        y = radius * np.outer(np.sin(u), np.sin(v)) + pos[1]
        z = radius * np.outer(np.ones(len(u)), np.cos(v)) + pos[2]

        ax.plot_surface(x, y, z, color=color, alpha=0.9, linewidth=0)
```

### Supercell Visualization

```python
from ase.build import make_supercell
import numpy as np

# Create 2x2x2 supercell for better visualization
matrix = np.diag([2, 2, 2])
supercell = make_supercell(atoms, matrix)

# Draw with reduced detail
draw_atoms(ax, supercell)
draw_bonds(ax, supercell, cutoff=2.5)
```

---

## Export Formats

Atomic visualizations use Matplotlib, so all standard formats are supported:

```python
# Vector formats (best for publications)
plt.savefig('structure.svg', format='svg')
plt.savefig('structure.pdf', format='pdf')

# Raster formats
plt.savefig('structure.png', dpi=300)
plt.savefig('structure.jpg', dpi=300, quality=95)
```
