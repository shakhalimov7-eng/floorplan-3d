# Floor Plan Interior Designer

A pure front-end tool for interior design on a floor plan: place furniture, remove or modify walls, and take measurements on a 2D plan, then switch to a Three.js 3D scene with one click — view it from above or walk through it in first person. The whole app is a single `index.html`: no build step, just open it.

## Features

**2D Floor Plan**
- Displays the floor plan at 1:60 / 1:100 scale, dimensions in mm
- Drag 60+ furniture and appliance items from the library on the left (bedroom, living room, dining & kitchen, bathroom, appliances, study & leisure)
- Move, rotate (hold Shift for free angle), and resize items, with automatic snapping to walls
- Measuring tool (snaps to nearby walls; hold Shift to lock horizontal / vertical)
- Remove or modify non-load-bearing walls; load-bearing walls are marked separately
- Layer toggles: dimensions, room names, furniture, grid, load-bearing walls

**3D Scene**
- Bird's-eye, oblique, and top-down views; click a room in the list to fly to it
- Walkthrough mode: WASD + mouse on desktop, virtual joystick on touch devices; click doors to open / close them
- Toggle between full-height and cut-away walls, time-of-day sunlight slider, night lighting
- Detailed furniture models: cabinet door gaps and handles, upholstered headboards, metal and ceramic materials with environment reflections, and more
- Select and drag furniture in 3D as well, kept in sync with the 2D plan in real time

**Plans & Statistics**
- Automatic calculation of room areas and net usable floor area
- Change the floor material of each room (wood, tiles, marble, terrazzo, carpet, etc.), with cost estimates in USD based on area plus 5% wastage
- Undo / redo; designs are auto-saved in the browser's local storage
- Uzbek / Russian / English UI (buttons on the right of the top bar; defaults to Uzbek and remembers your choice)
- Export to PNG, export / import designs as JSON
- Import your own floor plan as JSON (see below)

## Quick Start

```bash
git clone <repository-url>
cd <repository-directory>
```

Then simply open `index.html` in your browser. Alternatively, start a local static server:

```bash
python3 -m http.server 8000
# Visit http://localhost:8000
```

> Three.js is loaded from the jsDelivr CDN, so an internet connection is required the first time you open the 3D scene.

## Keyboard Shortcuts

| Key | Action |
| --- | --- |
| `T` | Toggle 2D / 3D |
| `V` / `M` / `X` | Select / Measure / Modify walls |
| `R` / `Shift+R` | Rotate selected furniture 90° clockwise / counterclockwise |
| `Delete` / `Backspace` | Delete selected furniture |
| `Ctrl/⌘ + D` | Duplicate selected furniture |
| `Ctrl/⌘ + Z`, `Ctrl/⌘ + Shift + Z` | Undo, redo |
| `F` | Fit to window |
| `+` / `-` | Zoom in / out |
| `[` / `]` | Show / hide the furniture library (left) and the side panel (right) |
| `Shift + F` | Fullscreen |
| `Esc` | Cancel current action |
| Walkthrough: `WASD` / arrow keys, `Shift`, `E` | Move, walk faster, open / close doors |

## Tech Stack

- Vanilla HTML / CSS / JavaScript — no framework, no build step
- 2D floor plan rendered with SVG
- 3D scene built with [Three.js](https://threejs.org/) r160 (OrbitControls, PointerLockControls, RoundedBoxGeometry, RoomEnvironment, CSS2DRenderer)
- Data stored in `localStorage`

## Using Your Own Floor Plan

Use **File ▾ → Import plan** with a JSON file that has a `plan` section (walls, windows, doors, sliding doors, openings, rooms, dimension chains) and an optional `furniture` list. 2D, 3D, areas and the cost estimate all follow the imported plan, and it stays saved in the browser until **File ▾ → Reset to default**.

- [shablon-reja.json](shablon-reja.json) — a small ready-to-import example (bedroom, bathroom, living room)
- [shablon-qollanma.md](shablon-qollanma.md) — field-by-field guide (in Uzbek)
- **File ▾ → Export plan JSON** now includes the `plan` section, so exporting the default design gives a complete full-size example

**From an image (AI):** **File ▾ → Plan from image (AI)** sends a PNG/JPG/TIFF floor plan to an
[ArchVec](../archvec) server (`PORT=8080 python scripts/run_local.py`, key `dev-key`), shows the recognized walls,
doors, windows and rooms over the image for review, and imports the result. Geometry comes from computer vision;
the optional AI step (OpenAI) only names rooms and reads dimension numbers. Without it the scale is an estimate.

The default plan itself lives in `index.html`:

- `WALLS` / `WINS` / `DOORS` / `SLIDES` / `OPENINGS`: walls, windows and openings
- `ROOMS`: room polygons, names, and default floor materials
- `DIMS`: dimension chains
- `MATS`: floor material names and unit prices (USD/m²)
- `LIB`: furniture library (type, name, default size, color)
- `buildFurniture()`: 3D models for each furniture type
- `I18N`: Uzbek / Russian translations, keyed by the English text

## Social Media

- X (Twitter): [@akokoi1](https://x.com/akokoi1)

## License

[MIT](LICENSE)
