# WP-CAD-3D
wp-cad-3d
# UNISEFE CAD 3D

**Version:** 0.0.1 Alpha
**CAD Engine:** UNISEFE CAD 3D v0.0.3 Alpha
**Platform:** WordPress
**Type:** Browser-based 3D CAD
**Interface:** HTML + SVG + JavaScript
**License:** MIT

---

## Overview

**UNISEFE CAD 3D** is a lightweight browser-based 3D CAD application packaged as a WordPress plugin.

The CAD engine remains self-contained and is loaded by WordPress without restructuring the original application.

The WordPress layer acts only as a lightweight container around the CAD environment.

The goal is to keep the CAD independent from the active WordPress theme and reusable across different websites.

The CAD interface includes its own toolbar, drawing environment, properties panel, 3D camera state, workplane, geometric calculation logic and internal RIKI Space.

---

# Architectural Contract

UNISEFE CAD 3D follows a defined internal architecture:

```text
Static HTML
    ↓
User Interface

RIKI Space
    ↓
Canonical BYTE / BIT state
Permanent CHAINS

JavaScript
    ↓
Thin runtime
Input
Interpretation
Projection
I/O

SVG
    ↓
Projection of canonical geometry
```

SVG is used as a visual projection layer.

It is not intended to be the canonical geometric model.

POINT and GUIDE_POINT are designed to close through the Delta Zero state before command geometry is committed.

Temporary guide tracking remains interaction state and does not become permanent drawing geometry.

---

# Main Features

## 2D / 3D Drawing Environment

The CAD includes drawing tools for:

* Line
* Dashed line
* Dash-dot line
* Circle
* Arc
* Text

These drawing commands operate inside the same CAD environment later used for 3D modeling.

---

# 3D Coordinates

The workspace uses three spatial coordinates:

```text
X
Y
Z
```

The HUD displays the current point as:

```text
x 0 · y 0 · z 0
```

The internal RIKI Space includes persistent coordinate values for:

```text
POINT_X
POINT_Y
POINT_Z
```

as well as input and target coordinates.

---

# 3D Modeling

The current CAD engine includes dedicated modeling commands for:

* Extrude
* Revolve

These commands extend the basic drawing environment into 3D geometry.

---

# Extrude

The **Extrude** command is designed to create geometry by extending selected geometry along a direction / normal.

The tool is available directly in the MODEL section of the main toolbar.

---

# Revolve

The **Revolve** command creates geometry by rotating the source geometry around an axis.

The command is part of the same native CAD environment and does not require an external modeling framework.

---

# 3D View

The CAD includes a dedicated 3D viewing system.

Available controls include:

* Orbit
* Fit
* Zoom
* Pan

The internal state includes dedicated camera values:

```text
CAMERA_YAW
CAMERA_TILT
```

These values remain inside the RIKI Space.

---

# Workplane

The CAD includes a persistent workplane concept.

Current state includes:

```text
WORKPLANE
WORK_Z
```

The default workplane is:

```text
XY
```

This provides a basis for creating geometry in a 3D environment while maintaining a defined construction plane.

---

# Dimensioning

The CAD includes native dimension tools for:

* 3D linear dimension
* Angular dimension
* Progressive aligned dimension

The 3D dimension tool is designed around:

```text
Two points
+
Axis / direction
```

Dimension values are connected to the internal geometric relations and RIKI state.

---

# Editing Tools

Available editing commands include:

* Select
* Undo
* Move
* Copy
* Trim
* Extend
* Fillet
* Mirror
* Rotate
* Delete

The editing environment is shared between drawing and modeling operations.

---

# View Controls

The drawing environment includes:

* Orbit
* Fit drawing to view
* Zoom
* Pan
* Dynamic camera orientation
* Cursor-centered interaction

The 3D camera state is stored persistently in the RIKI Space.

---

# Snap System

The CAD contains geometric snap modes for:

* Numeric snap
* Midpoint
* Quadrant
* Intersection
* Perpendicular
* Tangent

The 3D internal state also contains dedicated snap coordinates:

```text
SNAP_X
SNAP_Y
SNAP_Z
```

and target coordinates:

```text
SNAP_TARGET_X
SNAP_TARGET_Y
SNAP_TARGET_Z
```

---

# Geometric Relations

The engine includes permanent mathematical relations for geometric operations.

Examples include:

* Distance
* Delta X
* Delta Y
* Midpoint
* Angular relations
* Segment projection
* Perpendicular projection
* Line-line intersection
* Line-circle intersection
* Circle-circle intersection
* Tangent geometry
* View transformations
* Zoom transformations
* Fit calculations

These relations are stored as permanent RIKI chains.

---

# RIKI Space

The CAD contains an internal **RIKI Space** named:

```text
RIKI_CAD
```

The Space stores the canonical state of the CAD.

Examples of persistent values include:

```text
CONSTITUTION
SPACE
CAD
TOOL
SNAP
VIEW_X
VIEW_Y
VIEW_SCALE
CAMERA_YAW
CAMERA_TILT
WORKPLANE
WORK_Z
TEXT_SIZE
DIM_DECIMALS
LINE_COLOR
LINE_WIDTH
POINT
SNAP_POINT
DIMENSION
```

Each RIKI byte contains:

```text
NAME
VALUE
BIT
```

---

# 3D Point State

The internal point system includes:

```text
POINT_INPUT_X
POINT_INPUT_Y
POINT_INPUT_Z

POINT_TARGET_X
POINT_TARGET_Y
POINT_TARGET_Z

POINT_X
POINT_Y
POINT_Z

POINT_DELTA
```

This separates input state, target state and resolved point state.

---

# Guide State

Temporary geometric guidance uses a dedicated internal state:

```text
GUIDE_TARGET_X
GUIDE_TARGET_Y
GUIDE_TARGET_Z

GUIDE_X
GUIDE_Y
GUIDE_Z

GUIDE_DELTA
```

Guide tracking is intended for interaction only.

It is not automatically converted into permanent drawing geometry.

---

# Snap State

The snap system also maintains its own XYZ state:

```text
SNAP_POINT

SNAP_TARGET_X
SNAP_TARGET_Y
SNAP_TARGET_Z

SNAP_X
SNAP_Y
SNAP_Z

SNAP_DELTA
```

This allows geometric snap resolution to remain distinct from raw pointer input.

---

# Δ / BIT State

The CAD exposes the RIKI validation concept directly in the interface.

Example:

```text
POINT · Δ 0 · BIT 1
```

The general closure concept is:

```text
Δ = 0
BIT = 1
```

when the requested relation is considered closed according to the current state.

The internal Space also contains a native:

```text
SELF_TEST
```

byte.

---

# Permanent Drawing Region

Drawing geometry is stored inside a permanent RIKI region:

```html
<riki-region name="DRAWING" permanent="true">
```

Mathematical relationships are stored in a second permanent region:

```html
<riki-region name="CHAINS" permanent="true">
```

The intention is to keep geometry, state and relations inside one logical environment.

---

# Permanent Chains

The CAD includes several permanent chains.

Examples:

```text
DIST
DX
DY
MID_X
MID_Y
DIM_L
ANG_RAD
ANG_DEG
SNAP_D
HIT_D
VIEW_W
VIEW_H
ZOOM_SCALE
ZOOM_CENTER_X
ZOOM_CENTER_Y
FIT_SCALE
INT_X
INT_Y
PERP_X
PERP_Y
TAN_X1
TAN_Y1
TAN_X2
TAN_Y2
```

These chains describe mathematical relationships between the canonical values stored in the RIKI Space.

---

# Command State

CAD commands also have dedicated state bytes.

Examples include:

```text
CMD_SELECT
CMD_LINE
CMD_DASHLINE
CMD_DASHDOTLINE
CMD_CIRCLE
CMD_TEXT
CMD_DIM
CMD_ANGDIM
CMD_ALIGNDIM
CMD_MOVE
CMD_COPY
CMD_TRIM
CMD_EXTEND
CMD_FILLET
CMD_MIRROR
CMD_ROTATE
CMD_EXTRUDE
CMD_REVOLVE
CMD_DELETE
CMD_FIT
CMD_SAVE
CMD_OPEN
CMD_UNDO
```

This keeps command state within the same canonical Space.

---

# User Interface

The CAD interface includes:

* Top CAD toolbar
* White drawing / modeling canvas
* Native SVG projection surface
* XYZ coordinates HUD
* Command HUD
* Selection status
* Entity counter
* Sliding properties panel

The visual layout remains intentionally compact.

---

# Toolbar Groups

The current interface is divided into:

```text
DRAW
DIMENSION
EDIT
MODEL
VIEW
PROJECT
```

This keeps drawing, editing and 3D modeling tools inside a single workspace.

---

# MODEL Toolbar

The MODEL group currently contains:

```text
Extrude
Revolve
```

These commands form the first native solid-modeling layer of UNISEFE CAD 3D.

---

# VIEW Toolbar

The VIEW group currently contains:

```text
Orbit
Fit
```

Orbit controls the 3D camera orientation.

---

# PROJECT Toolbar

The project section includes:

* Open
* Save
* Print
* Save as PDF through browser printing

The CAD project can therefore remain independent from the WordPress database.

---

# Properties Panel

The properties panel currently contains settings for:

## Line

* Color
* Width

## View / Drawing

* Snap
* Midpoint snap
* Quadrant snap
* Intersection snap
* Perpendicular snap
* Tangent snap
* Text size
* Dimension decimals

---

# Responsive Interface

The interface includes layouts for:

* Desktop
* Tablet
* Mobile
* Small mobile screens
* Landscape mobile devices

On smaller displays, the properties panel becomes a slide-out panel.

This preserves as much modeling area as possible.

---

# WordPress Integration

The WordPress plugin intentionally keeps integration minimal.

WordPress is used mainly to:

1. Load the CAD.
2. Display the application inside a page.
3. Provide shortcode integration.

The internal CAD engine remains independent.

---

# Installation

Download the plugin ZIP.

In WordPress go to:

```text
Plugins
→ Add New Plugin
→ Upload Plugin
```

Upload:

```text
unisefe-cad-3d-0.0.1-alpha.zip
```

Activate the plugin.

---

# Shortcode

Insert UNISEFE CAD 3D into a WordPress page using:

```text
[unisefe_cad_3d]
```

An additional alias is available:

```text
[riki_cad_3d]
```

---

# Custom Height

The application height can be customized.

Example:

```text
[unisefe_cad_3d height="900px"]
```

or:

```text
[unisefe_cad_3d height="85vh"]
```

---

# Architecture

The plugin follows a deliberately simple architecture.

```text
WordPress
    │
    ▼
Plugin wrapper
    │
    ▼
UNISEFE CAD 3D
    │
    ├── Static HTML interface
    ├── SVG projection
    ├── CAD drawing tools
    ├── 3D modeling tools
    ├── Camera state
    ├── Workplane state
    ├── RIKI Space
    ├── Permanent CHAINS
    └── Persistent drawing state
```

The WordPress layer does not duplicate the CAD engine.

---

# Design Principle

The plugin follows the same principle as UNISEFE CAD 2D:

> WordPress hosts the CAD.
> WordPress does not become the CAD.

This keeps the application portable and reduces dependencies between the CAD engine, WordPress themes and other plugins.

---

# Canonical Geometry Principle

The visual SVG representation is only a projection.

The intended canonical state lives in the internal RIKI Space.

This separates:

```text
Canonical state
from
Visual representation
```

and allows view transformations to change without redefining the underlying logical CAD state.

---

# Portability

Because the main application remains self-contained, the engine can potentially run in:

* WordPress
* Static HTML
* GitHub Pages
* Local browser environments
* Other web applications
* Other CMS environments

Only the external wrapper needs to change.

---

# Current Status

**Alpha**

The project is under active development.

Current development areas include:

* 3D modeling refinement
* Extrude behavior
* Revolve behavior
* Camera / orbit behavior
* Native 3D geometry
* Workplanes
* Selection
* Snap behavior
* 3D dimensions
* Persistent state
* RIKI validation
* Structural integration

The Alpha designation means commands and internal structures may still change.

---

# Planned Evolution

Possible future development includes:

```text
UNISEFE CAD
    │
    ├── CAD 2D
    │
    ├── CAD 3D
    │
    ├── RIKI Section
    │
    ├── RIKI Connection
    │
    └── Structural
```

The objective is to keep a shared interface and a common RIKI state model across the different modules.

---

# Relationship with UNISEFE CAD 2D

UNISEFE CAD 3D continues the same general architecture used in UNISEFE CAD 2D.

The main difference is the introduction of:

```text
Z coordinate
3D point state
3D snap state
Camera yaw
Camera tilt
Workplane
Orbit
Extrude
Revolve
```

This makes the 3D application an extension of the same CAD concept rather than a completely separate environment.

---

# Philosophy

UNISEFE CAD 3D is being developed around a lightweight, transparent and browser-native approach.

The project attempts to keep:

```text
Geometry
State
Relations
Commands
View
```

inside one inspectable environment.

The purpose is not only to display three-dimensional geometry, but to preserve the relationships and state that define it.

---

# Technical Notes

Current technologies include:

```text
HTML5
CSS
JavaScript
SVG
WordPress / PHP wrapper
```

No external 3D CAD framework is required by the current application.

The current visual projection is handled through SVG.

---

# Repository Structure

Typical plugin structure:

```text
unisefe-cad-3d/
│
├── unisefe-cad-3d.php
│
├── app/
│   └── unisefe-cad-3d.html
│
└── README.md
```

The main CAD application remains inside the `app` directory.

---

# Version

```text
UNISEFE CAD 3D
WordPress Plugin 0.0.1 Alpha
CAD Engine 0.0.3 Alpha
```

---

# Author

**ITALFABER / UNISEFE**

Website:

https://italfaber.com/

---

# License

MIT License

The software may be used, studied, modified and redistributed according to the terms of the MIT License.

---

## Experimental Project

UNISEFE CAD 3D is an experimental CAD project under active development.

It should currently be considered an **Alpha software environment**, suitable for testing, experimentation and continued development.
