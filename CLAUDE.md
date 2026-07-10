# klipper-config

Klipper 3D-printer configuration. `.cfg` files only — no build, no tests.

## Hard rules

- **Never push to `main`/`master`.** Feature branch + PR, always. Open the PR,
  report the URL, stop — **only the human merges.**
- **Conventional Commits** (`feat:`, `fix:`, `chore:`, …). Never hand-bump a version.
- **`pre-commit` is the gate.** Run `pre-commit run --files <changed>` before
  declaring a change done, and report the result.
- **Never hand-edit the `SAVE_CONFIG` block** at the bottom of `printer.cfg`.
  Klipper generates and rewrites it; edits are silently overwritten.
- Plan every non-trivial task. If the plan fails, restart planning.

## Architecture

`printer.cfg` is essentially a list of `[include]`s. The real content lives in:

- `configs/` — one file per subsystem: `steppers`, `extruder`, `bed_heater`,
  `bed_mesh`, `beacon`, `fans`, `thermistors`, `resonances`, `shaketune`, `leds`,
  `gql_and_homing`, `main_printer`, `external_configs`
- `macros/` — `print_start`, `print_end`, `homing`, `park`, `pause_resume`, `g32`,
  `qgl_scan`, `sensorless`, `personal_macros`
- `AFC/` — Automated Filament Changer: `AFC.cfg`, `AFC_Hardware.cfg`, `macros/`, `mcu/`

`moonraker.conf`, `KlipperScreen.conf` and `timelapse.cfg` sit at the root.

## Commands

```bash
pre-commit run --files <changed files>
```

## Gotchas

- **The `SAVE_CONFIG` block in `printer.cfg` is machine-generated.** Do not touch it.
- The `klipper-config-check` pre-commit hook shells out to `python3 ~/klipper/klippy/klippy.py`,
  so it needs a local Klipper checkout at `~/klipper`. It is not portable to CI,
  and it passes vacuously where that checkout is absent.
- `.gitignore` also ignores `mise.toml` and `printer.cfg`; `printer.cfg` is
  force-added and tracked anyway.
- `gitleaks` runs on every commit — `moonraker.conf` and `KlipperScreen.conf` are
  the kind of file that grows an API key by accident.
- Personal, untracked notes belong in `CLAUDE.local.md`, which is gitignored.
