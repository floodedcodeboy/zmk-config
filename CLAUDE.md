# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A ZMK firmware configuration repo for two keyboards:
- **Lily58** — split keyboard on nice!nano v2 with nice!view ePaper displays, ZMK Studio enabled
- **Contra** — ortholinear 4x12 on nice!nano v2, ZMK Studio enabled

## Build System

Firmware is built via GitHub Actions only — there is no local build. Push to `master` triggers the workflow at `.github/workflows/build.yml`, which delegates to `zmkfirmware/zmk/.github/workflows/build-user-config.yml@main`. The build matrix is defined in `build.yaml`.

Download the compiled `.uf2` firmware artifacts from the GitHub Actions run page.

## Repository Structure

- `config/` — keyboard configuration files (keymaps and Kconfig)
  - `*.keymap` — devicetree overlays defining layers, behaviors, and key bindings
  - `*.conf` — Kconfig options (BLE, display, power, studio, etc.)
  - `west.yml` — West manifest pulling ZMK and third-party modules (zmk-nice-oled, zmk-oled-adapter from mctechnology17)
- `build.yaml` — defines the GitHub Actions build matrix (board/shield/snippet/cmake-args combos)
- `boards/shields/` — directory for custom shield definitions (currently empty)
- `zephyr/module.yml` — registers this repo as a Zephyr module with custom board root

## Key Conventions

- Keymap files use ZMK devicetree syntax (`#include <behaviors.dtsi>`, `#include <dt-bindings/zmk/keys.h>`, etc.)
- The Lily58 keymap uses custom behaviors: `mt` (mod-tap with balanced flavor, 350ms tapping term), `td0`/`td1` (tap-dance for bracket cycling)
- The Lily58 default layer uses home-row mods (SHIFT/CTRL/ALT/GUI on A/S/D/F and J/K/L/;)
- The Contra keymap defines layers via `#define` aliases (DEFAULT, WORKMAN, NUM_MODS, SHIFT_MODS, SYS_MODS)
- Reserved/extra layers in Lily58 are for ZMK Studio runtime editing
- ZMK Studio is enabled on both boards (`CONFIG_ZMK_STUDIO=y`, `CONFIG_ZMK_STUDIO_LOCKING=n`)

## Adding a New Keyboard

1. Add `<name>.keymap` and `<name>.conf` to `config/`
2. Add the board/shield combo to `build.yaml`
3. If it needs a custom shield definition, add it under `boards/shields/`

## External Dependencies (via West manifest)

- `zmkfirmware/zmk` (main) — core ZMK firmware
- `mctechnology17/zmk-nice-oled` — custom OLED widget module
- `mctechnology17/zmk-oled-adapter` — OLED adapter support
