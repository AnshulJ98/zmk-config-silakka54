# Modifier-architecture bake-off plan — revision 2

Status: LOCKED 2026-08-11 (owner approval). Phases 1+2 merged and shipped
together per owner — the combined state is the measured baseline. WM-layer
adjunct parked; HRM gate (WPM ≥ 70) agreed, opens a decision point, not an
automatic Phase 4.
Evidence: measured chord inventory (below) + deep community research
(full cited report: scratchpad/modifier-research.md, distilled here).
Owner corrections applied: Studio never touched (repo == board, no
graduation phase); sticky shift never adopted (the handoff's "empirical
datapoint favoring one-shots" was false); fixed points are QWERTY letters
+ number row ONLY.

## Measured chord inventory

- Aerospace: 46 chords — alt (26), alt+shift (20); rapid-repeat alt+hjkl,
  alt+1..0 and alt+shift+1..0.
- Kitty: 38 — cmd (17 incl. cmd+1..9), cmd+shift (9), cmd+opt/ctrl+arrows.
- Neovim: C-hjkl, C-S-hjkl, F1/2/3/5/7/8 (DAP), built-ins c-d/u/o/i/r/w.
- Browser: ctrl+tab, Vimium shift-capitals.
- Pattern: same targets (hjkl, 1..0) under a different modifier per app.

## Research findings that bind this plan

1. HRM misfires RISE below ~80 WPM; urob's own formula demands ~191ms
   require-prior-idle at 55 WPM — a value no published config uses. A
   vim-specific idle-gating failure is documented (`f(` → `fd9`). Even
   urob routes prose Shift through a thumb sticky key, not HRM.
   → Any HRM phase is gated on WPM recovery (≥70), else it tests a
   strawman.
2. Held `&sk` == held modifier, verified in v0.3.0 source. Rapid-repeat
   WM chords work under Callum-style. All needed behaviors are stock
   (`ignore-modifiers` default-on). Gotchas: hold-tap-wrapping-a-sticky
   must be `tap-preferred` (zmk#3280); `quick-release` kills mod-chaining
   so apply per-key only if the double-cap quirk appears.
3. No long-run tiling-WM + Callum report exists anywhere crawlable. The
   mechanical objection is dead; the cognitive one is untested. Owner
   skepticism is neither confirmed nor refuted by evidence.
4. Lydell (quit HRM, mods → number row) and Tratt (measured, mods → top
   row + dedicated Shift) both landed on DEDICATED mods placed
   deliberately. On 54 keys this is affordable and was absent from the
   original two-school framing. Both-hands access to each modifier is
   Tratt's stated hard requirement — our current layout violates it for
   Ctrl (right only) and Shift (left only).
5. "3-day dip" is unsourced; modifier-architecture changes take weeks to
   months. Phase windows therefore measure RELATIVE daily friction and
   fatal events, not final adaptation. Absolute misfire benchmarks do not
   exist in the literature; all metrics are within-owner comparisons.
6. 54-key positional trap: urob-style KEYS_L/KEYS_R lists that omit the
   number row silently break every alt+1..0 / cmd+1..9 chord. Our
   commented scaffold already includes rows 0-11 — keep it that way.
7. Thumb budget warning (Getreuer, multi-year reports): thumb overuse
   pain arrives after years; one frequent key per thumb is the safe load.
   Bias against adding layer-taps to Space/Backspace.

## Phases — one variable each, risk-ascending

### Phase 1 — instrumented baseline (now, ~5 days)
Current keymap + the uncontested Esc/Ctrl mod-tap on left outer home
(balanced, 280/175/190, hold-trigger = opposite hand + thumbs,
hold-trigger-on-release). Tap stays Esc. Start the friction log
(`kblog` alias → log/friction.md: timestamp, chord, what happened).
Purpose: baseline numbers + first taste of one dual-role key on a
non-alpha, low-ambiguity position.

### Phase 2 — coherent dedicated mods (~1 week, branch bakeoff/dedicated)
The missing candidate. No timers, no stickiness — reorganize the
dedicated modifiers the board already has into one coherent, mirrored
scheme satisfying: (a) both-hands access to Alt and Ctrl (the two
cross-app workhorses), (b) a recognizable anchor pattern replacing the
lost bottom-left cluster, (c) Cmd stays thumb (macOS chords are
overwhelmingly cross-hand already). Concrete arrangement designed at
phase start from the inventory; Esc position and outer columns are in
play (owner freed them). Cheapest cognitive change; directly tests
whether pain #1 is dispersion, not dedication.

### Phase 3 — Callum-style sticky mods (~1 week, branch bakeoff/callum)
cmd/alt/ctrl/shift as `&sk` on BOTH home rows of the Raise layer
(mirrored). Tap = one-shot, hold = plain hold (covers WM spam).
Multi-mod = tap both. Owner skepticism gets its controlled trial; the
tiling-WM evidence gap gets its first datapoint.

### Phase 4 — hybrid HRM (GATED: only when prose WPM ≥ 70, ~2 weeks)
Shift+Cmd as home-row holds (urob settings, prior-idle retuned to
10500/WPM at entry), Ctrl+Alt stay as Phase 3 stickies or Phase 2
dedicated keys (whichever won). Runs LAST both because risk is highest
and because the gate needs the retrain further along.

### Optional adjunct (phase-independent, separate decision): WM layer
A held layer emitting modified F-key chords (F13-F20 ceiling in
Aerospace, verified in its source), Aerospace re-bound to those. Deletes
all 46 alt-chords from the modifier problem entirely; solves nothing for
vim/cmd. Costs: two config sources of truth, laptop keyboard divergence,
F-key capture risk (use modified chords, never bare). Live precedent
exists (linkarzu's yabai layer). PARKED unless owner opts in.

## Measurement and decision rules

- Daily: `kblog` one-liners; nightly tally of misfires/hour-of-typing,
  wrong-modifier events, fatal events, subjective 1-5.
- FATAL EVENT (kills a phase immediately): unintended destructive action
  in vim/terminal from a dropped or spurious modifier (the `ctrl-d`→`d`
  operator-arming class).
- A phase "wins" by beating the baseline's steady-state friction while
  feeling no slower — owner-relative, since no external benchmarks exist.
- Promising-but-unstable at day 7 → extend to 14 before verdict
  (research says adaptation takes weeks; do not ship verdicts on day 3).
- Ties resolve toward fewer timers and fewer dual-role keys (every
  rigorous long-run account drifted that direction).

## Per-phase impact on current usage

### Phase 1 — zero keys move
- New: holding Esc = Ctrl. Route right-hand-letter ctrl chords (c-j/k/l,
  c-u/o/i) through it cross-hand; RCtrl stays for left-hand letters.
- Watch-item (log, don't fear): idle-then-fast `Esc j` overlap can
  resolve as ctrl-j (wrong window jump — annoying, not fatal). Prior-idle
  protects the common insert-exit case where Esc follows typing.

### Phase 2 — two keys change identity
- Top-right `-` becomes RAlt; bottom-right RAlt becomes RShift.
- Prepare: hyphen = Lower+O, underscore = Lower+P (both 2-chords, same
  cost as before). Tapping Alt where minus was is a harmless no-op.
- New patterns: mirrored anchors (corners: Alt top, Shift bottom); every
  alt+shift chord gets an opposite-hands pairing (LAlt+RShift or
  RAlt+LShift); left-hand capitals via RShift (kills the Vimium cramp).

### Phase 3 — nothing removed, one path added
- Base stays. Raise left home becomes sticky Shift/Ctrl/Cmd/Alt
  (pinky→index); Raise media drops to the bottom row.
- The gesture to drill: Raise → tap mod → release → key (alt+j = Raise,
  F, release, j). Spam variant: Raise+F held = held Alt.
- Dedicated rails remain as fallback; the measurement is whether the
  unified path displaces them voluntarily.

### Phase 4 — two dual-role pairs on home row (gated, WPM ≥ 70)
- F/J hold = Shift, D/K hold = Cmd. Capitals leave the pinkies entirely;
  cmd chords go cross-hand off the home row.
- Entry ritual: retune require-prior-idle to 10500/current-WPM; fatal
  -event rule armed; two-week window.
- Gate semantics: reaching 70 WPM opens a decision point — Phase 4 runs
  only if the Phase 2/3 winner left friction worth this risk tier.

## Rejected / parked (with reasons)

- Shift mod-morphs (,;/.:) — break vim indent and TS generics on this
  layout. REJECTED.
- Bracket combos, leader key module — redundant/no need. PARKED.
- Cmd/Lower thumb fumble — keycap differentiation only; every winning
  architecture relocates or demotes the dedicated Cmd thumb anyway.
- Miryoku wholesale — second retrain; its two stealable mechanisms
  (mods-as-plain-keys-on-layers, thumb repeat tradeoff) are already
  embedded in Phases 3/4 design.
