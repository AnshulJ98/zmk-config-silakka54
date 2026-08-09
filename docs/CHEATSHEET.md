# Silakka54 cheat sheet

## Daily facts

- Left half = central: owns the Mac connection (USB or BLE) and runs the
  whole keymap. Right half only streams key positions to the left.
- Right half's USB-C is charge-only in normal use; it becomes a flash drive
  only in bootloader mode.
- Red LED = charging. Battery % shows in the macOS Bluetooth menu when
  connected over BLE.
- Deep sleep after ~30 min idle; first keypress wakes it with a short delay.
- Power switches: off for transport, otherwise leave on.

## Studio vs repo (the workflow)

Repo = truth, Studio = scratchpad. Studio edits live in on-board settings
and OVERRIDE the compiled keymap — they survive reflashes and are one
settings-reset away from gone.

Weekly graduation: experiment in Studio during the week → port keepers into
`config/silakka54.keymap` → push (CI builds + re-renders the SVG) → flash
the left half → Studio: Restore Stock Settings to clear the overlay.

- Unlock Studio: **Raise+4** or **Lower+top-left corner** (all left hand).
  It relocks on disconnect and after idle.
- Studio labels shifted symbols by their base key (`&` shows as `7`).
  Not a bug. The SVG (`keymap-drawer/silakka54.svg`) shows real glyphs.
- Keymap changes only ever need the LEFT half flashed; the right half needs
  reflashing only if the shield itself changes.

## Flash procedure

1. Grab the `firmware` artifact from the latest green Actions run.
2. Plug the half in, double-tap its reset button (two crisp presses,
   under half a second apart) → `NICENANO` drive mounts.
3. Drag the UF2 on: `silakka54_left…` → left, `silakka54_right…` → right.
   It flashes, unmounts, and reboots itself. A `cp` I/O error at the very
   end is the success signature, not a failure.

Flashing never touches the bootloader — a bad flash is always recoverable
by double-tapping again.

## Troubleshooting

| Symptom | Fix |
|---|---|
| Right half types nothing | Split bond broken: flash `settings_reset` to BOTH halves, then real firmware to both, power both on together — they re-bond automatically |
| Mac won't pair / BLE flaky | Remove the device in macOS Bluetooth settings, Raise+5 (BT_CLR) on the board, re-pair. Profiles: Raise+1/2/3, USB↔BLE toggle: Raise+corner |
| No drive on double-tap | Timing — two crisp taps; try a direct port instead of a hub |
| Studio stuck on "Unlock To Continue" | Unlock chord above, over USB, in Chrome. If Raise is dead, the right half is dead — see row one |
| Symbols/backtick wrong in macOS | Keyboard was auto-identified as ISO: System Settings → Keyboard → Change Keyboard Type → ANSI |
| Roll back to vendor firmware | Bootloader → drag that half's `backup/vendor/<half>/CURRENT.UF2` (left to left, right to right only), then the settings-reset pairing dance |

## Version discipline

`config/west.yml` (ZMK source) and the `uses:` ref in
`.github/workflows/build.yml` are pinned to the same release (`v0.3.0`).
They drift independently upstream — bump them together, never separately.
