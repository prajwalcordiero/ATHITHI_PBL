# 3D Indoor Navigation System

A browser-based 3D indoor navigation demo for a two-floor building. Pick a starting room and a destination room, and the app draws the route through corridors and stairs, points you the way, tracks your position live, and lets you simulate walking with the keyboard.

Built with plain HTML, CSS and JavaScript plus [three.js](https://threejs.org/) (r128). No build step, no dependencies to install.

## Features

- **Two floors** (Ground and First) joined by a staircase, with 16 named rooms
- **Start and destination selection** from dropdowns; choosing a start places you in that room
- **Shortest-path routing** (Dijkstra) across a waypoint graph, including floor changes via the stairs
- **Route visualisation**: green path on the floor, a red beacon at the destination, amber beacons at the stairs, and a 3D arrow above you pointing to the next waypoint
- **Live tracking**: current floor, current room (or corridor), distance remaining, and turn-by-turn text such as "Turn left, then 6 m"
- **Automatic re-routing** from wherever you are, so a wrong turn is corrected on the fly
- **Keyboard simulation** with wall collisions, plus on-screen buttons for touch devices
- **Auto-walk mode** where the avatar follows the route by itself (good for presentations)
- **2D minimap** showing the current floor, your heading and the route
- **Two camera modes**: chase camera (drag to orbit, scroll to zoom) and a top-down overview
- Light and dark theme support

## Getting started

1. Download `indoor-nav-3d.html`.
2. Open it in any modern browser (Chrome, Edge, Firefox, Safari).

An internet connection is needed the first time because three.js is loaded from the cdnjs CDN. To run it fully offline, download `three.min.js` (r128) and change the `<script src="...">` tag to point at your local copy.

You can also serve the folder with any static server, for example:

```bash
npx serve
```

## Controls

| Action | Keys |
| --- | --- |
| Move forward / backward | `↑` / `↓` or `W` / `S` |
| Turn left / right | `←` / `→` or `A` / `D` |
| Take the stairs | `E` or `Space` (when standing near the Staircase) |
| Orbit camera | Click and drag |
| Zoom camera | Mouse wheel |

Pressing a movement key turns Auto-walk off. On touch devices, use the on-screen arrow buttons and the "Take stairs" button.

## How to demo it

1. Choose a **Start** (for example Reception) and a **Destination** (for example Conference Room on the First floor).
2. Press **Navigate** and follow the green line and the arrow.
3. Walk with the keyboard, or press **Auto-walk** to let the avatar do it.
4. Near the Staircase, press **E** to change floors when the instruction tells you to.
5. Switch the camera to **overview** to show the whole route, or tick **Show both floors**.

## How it works

| Part | What it does |
| --- | --- |
| Room data (`NAMES`, `rooms`) | Defines the 16 rooms: 4 on each side of the corridor, on each floor |
| Graph (`nodes`, `adj`) | Every room has a centre node, a door node and a corridor node; the two staircase nodes are linked across floors |
| `dij()` | Dijkstra's shortest path between two nodes |
| `plan()` | Finds the nearest start node for your current position, runs the search, and trims waypoints you have already reached |
| `upd()` | Runs about 5 times per second: re-plans, updates the panel text and redraws the route |
| `walls` / `hit()` | Wall rectangles used both to draw the walls and to block movement |
| `frame()` | Main loop: input, movement, camera, rendering and the minimap |

## Customising

- **Room names**: edit the `NAMES` object near the top of the script.
- **Layout**: walls and the route graph are generated from the 4 x 2 room grid, so a different layout means editing the loops that build `rooms`, `walls` and the graph.
- **Walking speed and turn speed**: in `frame()`, change `6` (units per second) and `2.2` (radians per second).
- **Floor height**: change the `FH` constant.

## Limitations

- The layout is a fixed grid, not loaded from a floor plan file.
- Position is simulated. A real deployment would replace the keyboard input with live positioning, for example BLE beacons, Wi-Fi fingerprinting or QR code checkpoints, and feed that position into the same `av` object.
- There is a single staircase; the avatar changes floors instantly (with a short height animation) rather than walking up the steps.

## Ideas for next steps

- Load rooms and walls from a JSON or GeoJSON floor plan
- Add more floors and a lift
- Click a room in 3D to set it as the destination
- Voice guidance for the turn-by-turn instructions
- Real positioning with BLE beacons or QR checkpoints

## Tech

HTML, CSS, vanilla JavaScript, three.js r128 (cdnjs).