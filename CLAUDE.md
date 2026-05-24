# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Godot 4.6 3D platformer using GDScript, Jolt Physics, and the Forward+ renderer (D3D12 on Windows).

## Key paths

- `project.godot` — engine config, main scene registration, physics/renderer settings
- `scenes/player.gd` — the only script in the project; player movement + jump logic
- `scenes/player.tscn` — `CharacterBody3D` with capsule mesh/collision, binds `player.gd`
- `scenes/coin.tscn` — `StaticBody3D` with cylinder collision, uses `assets/model/coin_new.glb`
- `scenes/level_1.tscn` — main scene (registered in project.godot); instantiates Player, Coin, floor, lights, camera
- `assets/model/` — 3D assets (GLB format with `.import` and `.res` generated files)

## Scene hierarchy

```
Level1 (Node3D)          ← main scene, uid://dks10hh17glg3
├── Floor (StaticBody3D)  ← 10×0.5×10 box with blue material and physics material
├── DirectionalLight3D
├── Camera3D              ← perspective, FOV ~74°, positioned behind/above origin
├── Coin (instance of coin.tscn)
└── Player (instance of player.tscn)
```

`player.tscn` is a `CharacterBody3D` composed of a `MeshInstance3D` (capsule) and `CollisionShape3D` (capsule shape). `coin.tscn` is a `StaticBody3D` with a `CylinderShape3D` collider and the `.glb` coin mesh (scaled 0.5).

## Player script (`scenes/player.gd`)

- `SPEED = 5.0`, `JUMP_VELOCITY = 4.5`
- Uses built-in `ui_*` input actions (arrow keys / WASD for movement, Space/Enter to jump)
- Standard `CharacterBody3D._physics_process` pattern: gravity, jump, input direction → `move_and_slide()`
- No camera attachment yet — camera is a separate node in the level

## Development

There is no CLI build/lint/test setup. All development is done through the Godot editor:
- Open the project in Godot 4.6+
- Press F5 to run the main scene, F6 to run the current scene
- Scripts are edited in Godot's built-in script editor or an external editor configured in Editor Settings

## File conventions

- `.godot/` is git-ignored (editor-generated cache)
- Scene files (`.tscn`) use `uid://` references, not path strings
- `.import` files are generated and should not be edited manually
- `.res` files are binary/imported resources
