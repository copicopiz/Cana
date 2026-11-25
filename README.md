## How?
The Series S build of the game natively on Windows PCs with
NVIDIA graphics cards, from game files you supply yourself. 
This project also provide Assetts Info of how formats work each other, so we can argue when pipelines needs atenttion

**This repository contains no game files or assets.** You need your own copy
of the game. Not affiliated with or endorsed by its developer or publisher.

## Status (2026-11-25)

Tested on Windows 11 with an **NVIDIA GeForce RTX 5050** (8 GB):

| | |
|---|---|
| Boot, First-boot screens, intro, opening credits | works |
| Prologue | Playable from the cell to the boat chase: cell, cafeteria, robbery, police chase (wanted level, minimap), boat, binoculars |
| Stability | 1 h 30 min+ in one session without a crash |
| Frame rate | 1-3 fps in the world (the first run is slower while shaders translate; they are cached after that) |
| Known problems | Anything in shadow is black and bright scenes overexpose (the game's ray-traced lighting does not run yet); some blocky artifacts; boats sit low in the water |

## Requirements

- Windows 10/11 x64, a CPU with AVX2
- A Direct3D 12 GPU with shader model 6.6 and mesh shaders (NVIDIA RTX 20 series or newer)
- Rust (the version in `rust-toolchain.toml`)
- Several GB of free Windows commit memory (raise the paging file if `crashes.log` reports error 1455)

## Build

```powershell
cargo build --release -p victory
```

## Play

```powershell
powershell -ExecutionPolicy Bypass -File scripts\play.ps1 -Game "your_game_folder\the_game.exe>"
(not working? try absolute game path folder)
```

`scripts\play.ps1` Keeps the shader cache and saves in folders you choose
(`-Cache`, `-Data`) And presses no buttons for you. Controller: XInput pads
(A DualShock/DualSense through DS4Windows works). With no pad connected the
keyboard acts as one while the game window has focus.

## Faster settings (optional, off by default)

`VICTORY_RELAY=2`, or `VICTORY_LATE_WAIT=3 VICTORY_WIDE_BACK=1` Cut frame
time on the test machines. Set them in PowerShell before running the script.

## Debugging switches

| Switch | Effect |
|---|---|
| `VICTORY_CAPTURE=<dir>` + `VICTORY_CAPTURE_FRAMES=<list>` | Save those frames as PNG (diagnostics build) |
| `VICTORY_KEYS=30:Enter,35:X` | Press keys at those seconds (diagnostics build) |
| `VICTORY_D3D12_DRED=1` | On a GPU failure, report the commands in flight |
| `VICTORY_PIPELINE_WAIT_MS=24` | Shorter per-frame wait for pipelines still being built |

Diagnostics build: `cargo build --profile diag -p victory --features diagnostics`

## License

GPL-3.0-or-later (see `LICENSE`). Based on VIctory (Marvisak) and victory-fork (fxceless).
