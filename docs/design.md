# Stage 0 layout review and rationale

Reviewed 2026-08-09 against the actual daily workflows: Neovim (kickstart,
nvim-dap), Aerospace (alt-driven, aerospace.toml read directly), Kitty
(ctrl+shift kitty_mod, native multiplexing), and terminal agents (Pi /
Claude Code). Every flow below was checked against the physical bindings,
not against intent.

## Defects found (fixed in Stage 0.1)

### 1. Shift+Tab was physically impossible
LShift (row 3, col 0) and Tab (row 1, col 0) are both left-pinky keys, and
there is no right Shift. Claude Code cycles permission modes with
Shift+Tab; shells use it for reverse completion. **Fix:** Lower+Tab sends
`LS(TAB)` — thumb and pinky are different digits, so the chord works.

### 2. Cmd+Space was physically impossible
Cmd and Space are both left-thumb keys with a key between them; one thumb
cannot press both. That kills Spotlight/Raycast. **Fix:** Raise+Space
(right-thumb layer hold + left-thumb Space) sends `LG(SPACE)` as one
binding. Cmd with any finger key (Cmd+C, Cmd+Shift+4, Cmd+Tab) was always
fine — the collision was only thumb-with-thumb.

### 3. No function keys anywhere
nvim-dap uses F7 (dap-ui) and F8 (dap-view) daily. **Fix:** Lower + number
row = F1-F10, aligned with the printed numerals. F11/F12 are omitted
(parked) — nothing in the current workflows uses them.

### 4. Held-Alt ergonomics were mediocre for an Aerospace-driven desktop
Alt existed only at the right-pinky bottom corner. `alt+hjkl` (focus, used
constantly) forced a same-hand pinky hold under the very fingers doing the
tapping. **Fix:** the base grave key (top-left corner) is now **LAlt**, so
`alt+hjkl` is a cross-hand chord. RAlt stays at the right corner so
`alt+shift+hjkl` and `alt+shift+1..0` remain cross-hand (LAlt+LShift would
be the same pinky). Grave lives on Lower+G; Tilde added on Lower+T.

## Second pass (full sweep, remaining known limitations)

| Flow | Verdict |
|---|---|
| `alt+1..5` / `alt+6..0` | Cross-hand via RAlt / LAlt respectively — pick the opposite-side Alt |
| `alt-shift-minus/equal` (resize) | Still impossible (RAlt and minus share the pinky; equal not on base). Use `alt-r` resize mode. Parked. |
| `ctrl+shift+<letter>` (kitty_mod) | RCtrl + LShift cross-hand, fine |
| `ctrl+hjkl` / `ctrl+d,u,o,i` (vim) | Works; same-hand pinky stretch for right-side letters. Acceptable until home row mods land (week 3), which put Ctrl on the home row and dissolve this |
| `cmd+enter`, `alt+enter` (new kitty) | Cross-hand thumb chords, fine |
| Shifted punctuation (`:` `"` `_` `<` `>`) | LShift + right-hand key, all cross-hand |
| `+` | Was a 3-key chord; now Lower+I, directly above `=` on Lower+K |
| `<` | Added on Lower+H; `>` on Lower+L. Generics stop requiring Shift+comma |
| `~` | Lower+T (above backtick on Lower+G) |
| BT_CLR placement | Moved from the minus position (adjacent to live nav muscle memory — one slip while holding Raise nuked the Mac pairing) to Raise+5, inside the deliberate left-hand BT cluster |
| Studio unlock | Raise+4 and Lower+grave-corner. The second one is all-left-hand: Studio is exactly the tool needed when the right half misbehaves, so unlock must not depend on the split link |
| Deep sleep | `CONFIG_ZMK_SLEEP=y`: first keypress after ~30 min idle takes a beat to wake — expected, not a fault |
| No right Shift | Left-pinky Shift for everything. Livable; HRM at week 3 adds Shift to the right home row. Parked |

## Studio "not showing properly" — likely explanations, in order

1. **Transparent keys.** Lower/Raise are mostly `&trans` by design; Studio
   renders those as near-empty keys. A sparse layer is correct, not broken.
2. **macOS keyboard-type misidentification.** If macOS ran Keyboard Setup
   Assistant and guessed ISO, the grave/backslash region types `§`/`±` and
   several symbols land wrong. System Settings → Keyboard → Change
   Keyboard Type → ANSI. This also masquerades as "layers are wrong".
3. **Geometry defects in the authored physical layout** — possible, since
   it was derived from Lily58 coordinates; needs a screenshot to judge.

## Editing tools verdict

- **ZMK Studio** is the only live on-board editor for ZMK, full stop. Its
  role here is scratchpad; the repo is truth (see README).
- **nickcoutsos keymap-editor** (https://nickcoutsos.github.io/keymap-editor)
  edits the repo itself through the GitHub API and renders from
  `config/silakka54.json`. This repo's structure (shield under
  `config/boards/shields/`, layout JSON in `config/`) was chosen for it.
  Changes land as commits → CI builds → flash. Better than Studio for
  anything meant to survive.
- **QMK's VIA/Vial equivalents do not exist for ZMK** — the wired RP2040
  Silakka54 has Vial, the wireless ZMK one never will.

## Practice tools (live typing on the new layout)

- `kitty +kitten show-key` — live keycode echo in the terminal already
  installed; instant verification of any chord, including layer keys.
- keybr.com — adaptive letter drills for the columnar-stagger retrain.
- monkeytype.com — custom text mode; paste TypeScript snippets to drill
  the Lower layer rolls (`=>`, `??`, `()`).
- typing.io — real source code including symbols; the closest thing to
  "practice the Lower layer on production-shaped input".
- The keymap SVG (`keymap-drawer/silakka54.svg`) on the second monitor is
  the reference while drilling.
