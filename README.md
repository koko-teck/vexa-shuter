# VEXA

**Voxel Tactical Unit** — a blocky/voxel tactical FPS set in a dusty desert military theatre called **Dust Town**.

- **Engine:** Godot 4.7.2.stable (`ed1daf0bf`)
- **Language:** GDScript
- **Target:** PC Windows — native executable, **not** a web/HTML5 build

---

## Running the project

The Godot editor is kept locally at `tools/Godot_v4.7.2-stable_win64.exe` so the
project is self-contained. It is a **tool**, not a game asset.

Open the editor:

```powershell
& "V:\vexa\tools\Godot_v4.7.2-stable_win64.exe" --path "V:\vexa" --editor
```

Run the game directly:

```powershell
& "V:\vexa\tools\Godot_v4.7.2-stable_win64_console.exe" --path "V:\vexa"
```

Headless smoke test (no window — useful in CI / after each change):

```powershell
& "V:\vexa\tools\Godot_v4.7.2-stable_win64_console.exe" --headless --path "V:\vexa" --quit-after 40
```

A healthy run prints the boot banner and exits with code `0`:

```
==================================================
 VEXA - Voxel Tactical Unit
 Godot 4.7.2-stable (official)  (major=4 minor=7)
 Boot scene: res://scenes/main.tscn
==================================================
```

---

## Exporting for Windows

VEXA targets **native PC Windows**. There is no web/HTML5 export preset and none may be
added.

Export a release build:

```powershell
& "V:\vexa\tools\Godot_v4.7.2-stable_win64_console.exe" --headless --path "V:\vexa" `
    --export-release "VEXA - Windows" "V:\vexa\build\windows\VEXA.exe"
```

Export a debug build:

```powershell
& "V:\vexa\tools\Godot_v4.7.2-stable_win64_console.exe" --headless --path "V:\vexa" `
    --export-debug "VEXA - Windows" "V:\vexa\build\windows\VEXA.exe"
```

Output: `build/windows/VEXA.exe` — a self-contained 64-bit Windows GUI executable
(`MZ`/PE header, machine `0x8664`, subsystem `2`). The PCK is embedded, so the single
`.exe` runs standalone with no browser and no external runtime.

Requirements: Godot export templates for `4.7.2.stable` must be installed at
`%APPDATA%\Godot\export_templates\4.7.2.stable\` — with the template files directly in
that folder (not nested inside a `templates/` subfolder).

The `tools/` editor and the `imagenes-de-referencia-/` reference folder are both excluded
from the build.

---

## Folder structure

```
vexa/
├── project.godot           Godot project config (identity, window, renderer)
├── icon.svg                VEXA icon — voxel chevron on sandstone
├── ART_DIRECTION.md        AUTHORITATIVE visual contract (read before making art)
├── README.md               this file
├── imagenes-de-referencia-/ reference images — REFERENCE ONLY, never shipped
├── tools/                  local Godot editor (not part of the game)
│
├── scenes/                 .tscn scene files, by domain
│   ├── main.tscn           boot scene (res:// scenes/main.tscn)
│   ├── gameplay/  player/  weapons/  inventory/  loot/
│   └── maps/  ai/  audio/  ui/  hud/  menus/  settings/  save/
│
├── scripts/                GDScript, mirrors scenes/ by domain
│   ├── core/               engine bootstrap & cross-cutting logic
│   ├── player/  weapons/  inventory/  loot/  maps/  ai/
│   ├── audio/  ui/  hud/  menus/  settings/  save/
│   ├── networking/         reserved — architecture prepared, nothing implemented
│   └── optimization/       reserved — pooling, LOD, culling helpers
│
├── assets/                 art content, by type
│   ├── characters/  weapons/  buildings/  props/  environment/
│   └── ui/  materials/  textures/  audio/  vfx/
│
├── resources/              Godot .tres data resources (data ↔ logic separation)
│   └── weapons/  player/  loot/
│
└── addons/                 third-party Godot plugins (currently empty)
```

**Why two parallel trees:** `scenes/` holds *composition* (nodes, transforms, wiring) and
`scripts/` holds *behaviour*, mirroring the same domain names. `assets/` holds raw art and
`resources/` holds tunable `.tres` data. This keeps gameplay logic, presentation, and data
separable — new weapons, maps, classes and modes get added without touching existing systems.

---

## Architecture

VEXA is built as a set of **modular domain boundaries** rather than one growing script per
system. The goal is that gameplay logic, presentation and data stay separable, and that the
project can grow new weapons, maps, classes and modes without rewriting what already works.

### The three layers

```
  ┌──────────────────────────────────────────────────────────┐
  │  PRESENTATION     ui/  hud/  menus/  audio/  vfx        │  reads events, never drives gameplay
  └───────────────────────────▲──────────────────────────────┘
                              │  GameEvents (signal bus)
  ┌───────────────────────────┴──────────────────────────────┐
  │  GAMEPLAY         player/  weapons/  inventory/  loot/   │  owns rules and state
  │                   maps/  ai/                              │
  └───────────────────────────▲──────────────────────────────┘
                              │  Game.register_service()
  ┌───────────────────────────┴──────────────────────────────┐
  │  DATA             resources/*.tres  +  *Config.gd        │  tunables live here, not in code
  └──────────────────────────────────────────────────────────┘
```

### Autoloads (two, deliberately)

`project.godot` declares exactly two autoloads, in a deliberate order:

| Order | Name | Script | Why |
|---|---|---|---|
| 1 | `GameEvents` | `scripts/core/game_events.gd` | The global signal bus. Loads first because every other system produces or consumes events. Stateless, dependency-free. |
| 2 | `Game` | `scripts/core/game.gd` | The service registry. Loads second because systems register into it during their own `_ready()`. |

Domain subsystems are **not** autoloaded. Each is instantiated by the scene that owns it and
registers itself, which keeps them testable in isolation and keeps the global namespace small.

> Note: `GameEvents` and `Game` intentionally have **no `class_name`** — GDScript forbids a
global class from sharing a name with an autoload singleton, because the autoload *is* the
global identifier.

### How systems talk to each other

**1. Signals, not references.** Gameplay emits on the bus; presentation listens. The HUD never
reaches into the player; the player never knows the HUD exists. This is what keeps rule 19
(gamplay ↔ UI separation) structural rather than a convention.

```gdscript
# producer (gameplay)          # consumer (HUD)
GameEvents.actor_damaged.emit(...)      GameEvents.actor_damaged.connect(_on_damaged)
```

**2. Service registry, not `get_node()` paths.** A system registers under a domain key;
others resolve it by name. Missing services degrade gracefully (`null` + warning) instead of
crashing, so the project stays runnable while later instructions are still being built.

```gdscript
Game.register_service(Game.SERVICE_AUDIO, audio_manager)
var audio := Game.get_service(Game.SERVICE_AUDIO)   # null-safe
```

Domain keys are declared as constants on `Game` (`SERVICE_PLAYER`, `SERVICE_WEAPONS`,
`SERVICE_AUDIO`, …) so names can never drift.

### Contract classes (the domain boundaries)

These are **contracts, not implementations**. They pin down the shape each system must have so
that later phases — and a future server-authoritative pass — plug into a defined interface.

| Class | Base | File | Real implementation |
|---|---|---|---|
| `GameMode` | `Node` | `core/game_mode.gd` | Instructions 89 (BR), 93 (S&D) |
| `Actor` | `CharacterBody3D` | `core/actor.gd` | Instructions 33–35 |
| `Session` | `Node` | `core/session.gd` | Match lifecycle (thin coordinator) |
| `NetworkSession` | `Node` | `networking/network_session.gd` | Future multiplayer — **boundary only** |
| `WeaponData` | `Resource` | `weapons/weapon_data.gd` | Instruction 50 (stats as data) |
| `PlayerConfig` | `Resource` | `player/player_config.gd` | Instructions 27–28 |
| `AudioConfig` | `Resource` | `audio/audio_config.gd` | Instructions 51–60 |
| `GraphicsConfig` | `Resource` | `settings/graphics_config.gd` | Instruction 108 |
| `ObjectPool` | `RefCounted` | `optimization/object_pool.gd` | Instruction 116 (working utility) |

### Constant domains (shared vocabulary)

Small pure-constant classes that stop the same concept being re-declared in several places.

| Class | Enforces / defines |
|---|---|
| `WeaponSlots` | **Exactly two** weapon slots (rules 42–43) — `COUNT = 2`, `PRIMARY`, `SECONDARY` |
| `LootRarity` | Common → Legendary, with **bounded** stat scaling so no tier dominates (rule 41) |
| `BotState` | Patrol / Search / Combat / Retreat / Dead (instruction 99) |
| `SurfaceType` | Sand / sandstone / concrete / wood / metal / fabric / vegetation, plus hard-vs-soft cover and per-material debris colours |
| `UILayers` | Canvas z-order for HUD, minimap, killfeed, menus, debug |
| `SaveKeys` | Every persisted setting key (instruction 115), so save/load can never mismatch |
| `GameModes` | Mode ids and display names for the lobby (instruction 103) |

### Data, not hard-coded values

Weapon stats, player tuning, audio levels and graphics options live in `Resource` classes so
they can be authored as `.tres` files and tweaked without touching scripts (rules 21–24).

### The no-giant-scripts rule

No GDScript file may exceed **300 lines**. Verified automatically — the largest script today is
well under that, and the check is part of the instruction-4 verification suite.

### Multiplayer readiness — without faking it

`NetworkSession` exists so the architecture is already shaped for a future online pass, but it
reports `OFFLINE` and `is_networked() == false`. Rule 85 forbids pretending multiplayer exists,
and rules 88–92 require everything to be playable offline. Nothing in the codebase assumes the
local instance is "the player" — actors are addressed by `peer_id`, and `0` means "local owner",
not "human".

### Verified

The architecture is verified by an automated suite run under headless Godot, covering: autoload
presence, all 18 signal-bus events, service register/resolve/unregister lifecycle, graceful
degradation for missing services, contract class base types, constant domains, the two-slot
rule, offline-only networking, and `ObjectPool` reuse/double-release safety.

> **Implementation note:** verify GDScript global classes with
> `ProjectSettings.get_global_class_list()`, **not** `ClassDB.class_exists()`. `ClassDB` only
> knows engine-native C++ classes and returns `false` for every GDScript `class_name`.

---

## Project identity

| Field | Value |
|---|---|
| Official name | **VEXA** |
| Genre | Voxel/blocky tactical shooter |
| Setting | Desert military — Dust Town |
| Faction | VEXA Tactical Unit |
| Teams | BLUE / ORANGE |
| Classes | Heavy, Infiltrator, Sniper |

All character, weapon, map and faction names are original to VEXA. No characters, logos,
maps or protected elements are copied from other video games.

---

## Art direction

**Read `ART_DIRECTION.md` before creating any asset.** It is the authoritative visual
contract and outranks personal preference.

Core rules, in brief:

- Voxel/blocky **always** — 1 block = 0.1 m; hard edges, no smooth curves, flat shading.
- Desert palette: sand beige, khaki, desert tan, coyote brown, olive drab, dark brown.
- No pure black, no pure white, no neon, no cool greys.
- Warm, powdery, low-sun lighting with tan haze and drifting dust.
- The reference images in `imagenes-de-referencia-/` are **consulted, never shipped** —
  they are never used as textures, sprites, skyboxes, backgrounds or models.

---

## Status

Development follows a fixed, ordered 116-instruction specification. Instructions are
implemented strictly one at a time and each is verified by running the project before the
next begins.

| Block | Instructions | Status |
|---|---|---|
| 1 — Project identity | 1–10 | In progress (1–4 done) |
| 2 — Assets & visual generation | 11–25 | Not started |
| 3 — Player | 26–35 | Not started |
| 4 — Weapons | 36–50 | Not started |
| 5 — Audio | 51–60 | Not started |
| 6 — HUD & minimap | 61–72 | Not started |
| 7 — Dust Town map | 73–88 | Not started |
| 8 — Game modes | 89–95 | Not started |
| 9 — AI & multiplayer prep | 96–100 | Not started |
| 10 — Main lobby | 101–106 | Not started |
| 11 — Settings | 107–115 | Not started |
| 12 — Optimization & delivery | 116 | Not started |

Completed so far: **Instruction 1** (project created), **Instruction 2** (engine & language),
**Instruction 3** (platform: native Windows only, verified by exporting and running a real `.exe`),
**Instruction 4** (modular architecture: signal bus, service registry, domain contracts and
data-driven configs — verified by an automated headless suite).
