# Earthquake Response Robot Simulation

A Unity simulation for testing earthquake search-and-rescue robots. It procedurally generates a city map, finds a route across it with A* and drives a robot along that route.

Built as a capstone project at **Bahçeşehir University** (Feb – Jun 2025) by an interdisciplinary team of computer and industrial engineering students.

## Why

Real disaster sites are dangerous and unpredictable, which makes them a poor place to develop and test rescue robots. This project provides a safe virtual environment where every run produces a new city layout, so navigation logic can be tested against many different scenarios.

## Features

- **Procedural city maps with Wave Function Collapse.** Each cell starts with every possible tile (roads, turns, intersections, grass, buildings). The cell with the lowest entropy is collapsed first using weighted randomness, and socket rules propagate to its neighbours, so roads always connect correctly.
- **Adjustable map size.** A slider sets the grid size before generation. Tile weights live in a `WeightsSO` asset and shape how the city looks.
- **Walkability export.** The finished map is exported as a `MapData` structure: a walkability grid plus per-tile direction flags that describe which ways a road can be travelled. Start and end points are picked automatically at a reasonable distance from each other.
- **A\* pathfinding.** The map is converted into a node grid and searched with A\* using a Manhattan-distance heuristic (four-directional movement), respecting each tile's allowed directions.
- **Robot movement.** The robot follows the computed path tile by tile and turns to face its direction of travel.
- **Robot camera.** Press `C` to toggle a picture-in-picture camera that follows the robot.
- **Low-poly assets.** Buildings, roads and the robot were modelled in Blender in a simple low-poly style.

## How it works

```
Generate Map (UI)
   │
   ▼
PrototypeGenerator ──► WaveFunctionCollapse ──► MapData
                                                  │  walkability + direction flags
                                                  ▼
                                    MapDataToPathfindingGrid
                                                  │
                                                  ▼
                                         AStarPathfinding
                                                  │  List<Vector3> path
                                                  ▼
                                           RobotMovement
```

| Script | Responsibility |
| --- | --- |
| `MapUI.cs` | Map size slider and *Generate Map* button; runs the whole pipeline from generation to robot movement |
| `PrototypeGenerator.cs`, `Prototype.cs` | Builds tile prototypes with their sockets and prefabs |
| `WaveFunctionCollapse.cs`, `Cell.cs` | Entropy-based collapse and constraint propagation |
| `Weights.cs`, `WeightsSO.cs` | Tile weight configuration |
| `MapData.cs` | Exported walkability grid, direction flags, start and end points |
| `MapDataToPathfindingGrid.cs`, `PathfindingGrid.cs`, `GridNode.cs` | Converts the map into a pathfinding grid |
| `AStarPathfinding.cs` | A\* search with a Manhattan heuristic |
| `RobotMovement.cs` | Moves and rotates the robot along the path |
| `RobotCamera.cs` | Toggleable follow camera |

## Running the project

1. Install **Unity 6** (`6000.0.40f1`) through Unity Hub.
2. Clone this repository and open the `My project (1)` folder in Unity Hub.
3. Open `Assets/Scenes/Main.unity`.
4. Press **Play**, choose a map size with the slider and click **Generate Map**.
5. Press `C` to toggle the robot camera.

## Tech stack

Unity 6 · C# · TextMeshPro · Blender

## Roadmap

The project report also designs an earthquake event system that is not implemented in this version:

- An initial earthquake event with screen shake, debris particles and sound, with a magnitude slider.
- Roadblock events that swap roadside buildings for collapsed versions and mark those roads as blocked, so A\* has to re-route the robot.

## Team

| Area | Responsible | Support |
| --- | --- | --- |
| User interface | Orhun Aslan | Kanan Nasibli |
| Physics & movement | Muhammed Said Hanlıoğlu | Orhun Aslan |
| Map generation | Mehmet Atıf Ertugay | Kanan Nasibli |
| 3D assets | Kanan Nasibli | |
| Pathfinding algorithm | Ufuk Sipahier | Fatma Subaşı, Miray Bal |
| Event control | Kanan Nasibli | Mehmet Atıf Ertugay, Orhun Aslan, Muhammed Said Hanlıoğlu |

Advisors: Assist. Prof. Barış Özcan (Computer Engineering) and Assist. Prof. Ayşe Kavuşturucu (Industrial Engineering).
