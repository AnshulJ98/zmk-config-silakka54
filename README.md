# zmk-config-silakka54

ZMK firmware for a Silakka54 MX Wireless (PandaKB build): nice!nano-v2-class
nRF52840 controllers, left half central, 54 keys (6x4 + 3 thumbs per half).
This repo is the source of truth for the keymap; ZMK Studio is a scratchpad
whose experiments graduate into `config/silakka54.keymap`.

## Repo layout

| Path | What it is |
|---|---|
| `build.yaml` | CI build matrix: left (with Studio), right, settings-reset |
| `config/west.yml` | Pins ZMK to `v0.3.0` — the firmware version is versioned here |
| `config/silakka54.keymap` | The keymap. Three layers + commented week-3 home-row-mod scaffold |
| `config/silakka54.conf` | Firmware options (deep sleep) |
| `config/silakka54.json` | Physical layout JSON for keymap-drawer and nickcoutsos keymap-editor |
| `config/boards/shields/silakka54/` | The shield: matrix wiring, transform, Studio physical layout |
| `keymap-drawer/silakka54.svg` | Auto-rendered keymap diagram (committed by CI on every keymap push) |
| `backup/vendor/` | CURRENT.UF2 readbacks of the shipped vendor firmware, per half |

The shield is derived from upstream ZMK's Lily58 (MIT): the Silakka54 PCB is
electrically a Lily58 minus four switches, so the wiring is identical and the
matrix transform drops the four positions that do not exist.

## Flashing

Firmware never touches the UF2 bootloader, so a bad flash is recoverable by
flashing again. Per half:

1. Plug the half into the Mac via USB-C. Double-tap its reset button; a
   `NICENANO`-style drive mounts.
2. Drag the UF2 onto the drive. It unmounts and reboots itself.

Which UF2 goes where (from the `firmware` artifact of the latest green
[Actions run](../../actions)):

- `silakka54_left nice_nano_v2` → **left** half (central; has Studio support)
- `silakka54_right nice_nano_v2` → **right** half
- `settings_reset nice_nano_v2` → either half, only during recovery (below)

Flash right first, then left, then power-cycle both.

## Halves won't pair / Bluetooth recovery

Bonds live in a settings partition that firmware flashes do not erase.
When pairing breaks:

1. Flash `settings_reset` to **both** halves (it has Bluetooth disabled, so
   they cannot re-bond mid-procedure).
2. Flash the real left/right UF2s back.
3. Power both on near each other; they bond automatically. Re-pair the Mac
   (Raise + LAlt-corner cycles USB/BLE output; Raise + 5 clears the active
   Bluetooth profile).

## Vendor firmware rollback

PandaKB publishes no firmware for this board; the only copies are the
readbacks in `backup/vendor/`. To roll back: enter the bootloader and drag
that half's `CURRENT.UF2` back on (left file to left half only, right to
right — the UF2 family ID is per-unit). Bonds are not in the dump; run the
settings-reset pairing procedure afterwards.

## Studio vs repo workflow

Studio edits live in on-board settings storage and override this repo's
compiled keymap. Experiment freely in Studio during the week; anything worth
keeping gets ported into `silakka54.keymap` and flashed, then the Studio
override is cleared (Restore Stock Settings in Studio, or settings-reset).
An unported Studio edit is one settings-reset away from gone.
