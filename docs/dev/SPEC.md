# System specification

This specification defines the complete intended desktop browser Tetris game. [PROJECT](PROJECT.md) owns purpose and scope; [ARCHITECTURE](ARCHITECTURE.md) and [DECOMPOSITION](DECOMPOSITION.md) own structural responsibilities. Requirement IDs below are stable traceability groups. Detailed geometry and configuration each have one canonical child owner.

## R01 — Board, pieces and spawning

C-GEOMETRY/C-BOARD/C-GAME implement the coordinate, mask, spawn and placement contracts in [GEOMETRY](spec/GEOMETRY.md). The visible grid has 10 columns and 20 rows. Transient cells may occupy two hidden rows above it. Settled cells exist only in the visible grid. A new game starts empty with score/lines/bonuses zero, level one, no held piece and a fresh source. Spawn collision causes game over. Locking with any cell above the visible grid causes game over without a partial placement or line-clear award.

Acceptance: enumerate all seven spawn masks, both walls, floor and hidden-row limits. A visible spawn collision and a grounded piece above row zero each produce game over; neither corrupts settled cells.

## R02 — Keyboard and command ordering

Left/Right move one column; Down attempts one-row soft drop. Up or X rotate clockwise; Z rotates counterclockwise; Space hard-drops; C holds; B activates penetration; P toggles pause; Enter starts/restarts from ready/game over. Visible Start, Pause/Resume and Restart buttons provide equivalent lifecycle actions. Recognized keys are consumed only when handled by the game; keyboard events targeted at input/select/textarea/button or contenteditable elements do not issue game commands or prevent their native use.

Ignore native repeated keydown events. Left/Right/Down execute once on first keydown, repeat after 150 ms and then every 50 ms while held. Most recently pressed lateral direction wins when both are held; releasing it resumes the still-held opposite direction with a fresh initial delay. Rotations, hard drop, Hold, pause and penetration are edge-triggered. Clear held-key state on pause, blur, visibility loss, restart and teardown. Held keys must be released and pressed again after those boundaries. Input held across a lock must not spill repeat actions onto a new piece; clear repeats at spawn.

Before an intent at timestamp t, process active elapsed time through t. Thus an exactly due lock precedes that intent, which targets the resulting current piece. Serialize all intents in arrival order when timestamps match. C-CONTROLLER owns ordering and C-INPUT owns repeats.

Acceptance: verify equal-time lock/action ordering, no native-repeat acceleration, opposite-key release, form focus, held-key clearing and one activation per key press.

## R03 — Session lifecycle and suspension

Statuses are ready, running, paused and game-over. Ready displays an empty board and Start. Only running accepts gameplay commands or advances time. Start/new game creates a fresh session and preserves selected settings. Explicit Restart works from running/paused/game-over. Blur or hidden visibility automatically pauses a running session; returning does not resume it automatically. Pause retains board, active pose, source/Hold, totals, bonuses and remaining timers. Resume resets the browser time anchor; suspended elapsed time is discarded. Game over freezes gameplay and exposes Restart.

Mount starts at most one animation chain; unmount cancels it and removes listeners. Repeated restart does not create extra loops or listeners. Missing Canvas context produces a visible initialization error instead of starting an unusable loop.

Acceptance: pause for a simulated minute and resume with identical remaining lock/gravity time; blur clears keys; repeated mount/restart/teardown has one or zero active chains as appropriate.

## R04 — Gravity and locking

Gravity interval at level L is max(100, 1000 − 75 × (L − 1)) ms. A spawn starts a fresh interval. On each gravity event attempt one row downward. Successful soft drop does not change the scheduled gravity time; it awards one point per descended row. A failed soft drop awards none. Process elapsed time in chronological order, carrying residual time; render frame rate does not change the result. Large active-time deltas may be processed in bounded chunks without dropping elapsed time. Paused time is excluded under R03.

Each spawned piece captures the selected general and hard-drop lock policies. In full-interval mode, when a placement first becomes grounded, start a deadline using the current level's gravity interval. Grounded movement/rotation never extends it. Becoming airborne cancels it; regrounding starts a fresh interval. The captured interval is not recomputed during that contact. Failed down attempts before the deadline leave the piece active. At the exact deadline, validate and lock. Evaluate grounding immediately after successful movement, rotation, spawn, Hold replacement, activation and drop, rather than waiting for a later gravity event. With simultaneous gravity and lock events, process the lock first and initialize the new piece's schedule at that timestamp.

In failed-down mode, a gravity or soft-down attempt that cannot descend locks immediately; merely becoming grounded does not lock. Normal grounded means the next downward pose collides with a wall/floor/settled cell. Penetrator grounding uses R11. Rejected actions preserve deadlines. Activation replaces normal grounding evaluation and cancels an obsolete normal deadline. Hold replacement and restart create fresh timing.

Acceptance: test deadline −1/exact/+1 ms, movement while grounded, airborne escape/reground, several rendering partitions with equal total elapsed time, soft drop immediately before gravity, pause and both policies.

## R05 — Lock, clear, score and level

A valid lock atomically places all four cells, removes all complete rows simultaneously, compacts remaining rows downward and inserts empty rows at the top. Add line-clear points using the level immediately before the clear: 100/300/500/800 × level for 1/2/3/4 rows. Increase total lines; level is 1 + floor(total lines / 10). Add no points for an ordinary lock, rotation, Hold, rejected input or penetrator activation. No combo, spin or back-to-back scoring is required. Hard-drop points are specified in R09.

After scoring/bonus awards, spawn the next piece, refresh Hold eligibility and capture selected policies. A game-over lock above the board awards no line points or bonus and consumes no queued replacement. White locked penetrator cells participate in later ordinary collision/clearing exactly like other settled cells.

Acceptance: 1–4 simultaneous rows, adjacent/nonadjacent clears, row-zero clearing, a 9→10 lines threshold using the old level for points, and top-out with no partial mutation.

## R06 — Piece source and next preview

Use seven-bag randomization: uniformly shuffle one copy of each type using an injected random source, consume every item before creating the next bag. The source exposes at least the next piece without consuming it. Preview rendering never changes the source sequence. New game resets source state. A baseline independent-uniform source may be used only in the earlier bounded delivery scope defined by PLAN.

Acceptance: complete aligned groups of seven contain each type once, a seeded sequence is reproducible, lookahead is stable, and Hold/preview toggles preserve source consumption rules.

## R07 — Hold

If enabled, running and not penetrating, C holds once between locks. With empty Hold, store the current type and spawn the next source piece. With populated Hold, exchange types without consuming the source. Replacement uses its spawn orientation/position, fresh timing and the currently selected lock policies; Hold remains unavailable until a subsequent lock. Spawn collision causes game over. Rejected Hold changes nothing. Disabling Hold retains its stored type and spent eligibility; re-enabling it does not grant an extra use. Penetrator activation disables Hold until that piece locks or the session ends.

Acceptance: empty/populated Hold, second-use rejection, disable/re-enable without erasure, held-piece spawn collision and penetration followed by rejected Hold without spending another bonus.

## R08 — Ghost

Ghost uses the active piece's current orientation and the same landing query used by hard drop. It is presentation only. Always-hide suppresses it; always-show renders any legal visible-board landing; dynamic mode renders only if every projected cell has y ≥ 10. A projection spanning rows 9 and 10 is hidden. If no fully visible legal landing exists, suppress the ghost in all modes; if ghost and active piece coincide, draw the outline behind the active piece. Ghost is an outline/translucent cue distinguishable from settled/active cells, including white penetration. Recompute after any relevant board, pose or configuration transition.

Acceptance: projections wholly above, wholly below, straddling and exactly at the midline; no-result; active/ghost coincidence; matching normal and penetrator ghost/drop coordinates.

## R09 — Hard drop

When enabled and running, Space moves the active piece to its landing in one command and awards two points per descended row, including penetrator travel through occupied rows. Zero distance awards zero. A no-result query rejects without mutation. If captured hard-drop policy is immediate, validate/place/clear/spawn in the same command. Otherwise apply captured general policy: a newly grounded drop starts a full interval; an existing full-interval deadline is preserved; failed-down mode waits for a later failed down attempt. Repeated zero-distance hard drops cannot postpone locking. Disabled hard drop changes neither pose nor score.

Acceptance: distance/score, disabled/no-result paths, both general policies crossed with immediate/follow, zero-distance repetition, existing deadline and intervening occupied rows for a penetrator.

## R10 — Rotation and wall kicks

Use the explicit SRS masks, orientation transitions and ordered offset tables in [GEOMETRY](spec/GEOMETRY.md). Try candidates in order, accept the first legal candidate, and reject with no pose/timer mutation if none is legal. Normal pieces reject settled-cell overlap. Penetrators allow transit overlap but require board bounds and a legal landing at or below the resulting pose. O rotation leaves the footprint unchanged. Rotation alone earns no score.

Acceptance: every ordered transition for I and JLSTZ, wall/floor and obstruction fixtures, O no-op, all-candidate rejection and penetrator no-landing rejection.

## R11 — Penetrator transit and landing

B succeeds only while running, with bonus count > 0, an ordinary active piece and at least one fully visible legal non-overlapping landing at or below the current pose. It spends exactly one bonus and marks that piece penetrating; it cannot toggle off or spend twice. Missing prerequisites reject without changes. Penetration lasts until lock or session termination and cannot be transferred through Hold.

For a fixed pose orientation/x, enumerate downward origin rows from the current origin through the last row fitting above the floor. A legal landing has all four cells within rows 0–19 and no overlap. Select the greatest y among these legal placements, regardless of intervening occupied cells. A lower legal cavity may be separated by occupied cells. If none exists, report no result.

Gravity/soft drop advance one origin row at a time, even through overlap, until the selected legal landing. A penetrator is grounded only at that lowest legal landing; an intermediate overlap or an intermediate valid pocket is not grounding. At the final landing, failed-down/general/immediate policies apply exactly as in R04/R09. Lateral movement and rotation must respect x and transient top/floor bounds and retain a legal landing at or below the proposed origin; otherwise reject. Transit never removes settled cells. Validate lock again before any board write; an unexpected invalid lock ends the session with a visible error and leaves settled cells untouched.

Acceptance: a barrier with a legal lower cavity, an occupied floor with only a higher legal pocket, an overlapping transit pose, an all-blocked column, sideways/rotation loss of all landings, and lock delay at the final landing. Ghost/drop must agree exactly on the selected deepest pose.

## R12 — Bonus and color feedback

Exactly four lines from one valid lock award one bonus after any activation cost, capped at three. Fewer lines award none. A four-line clear by a penetrator may earn one. Restart resets to zero. Active and locked penetrator cells are white; regular I/O/T/J/L/S/Z cells use distinct cyan/yellow/purple/blue/orange/green/red fills, with no white regular fill. Display exactly one lightning symbol per bonus with an accessible numeric label, updated in the lock/activation snapshot. A zero count is readable even with no symbols.

Acceptance: award at counts 0/2/3, spend then earn, insufficient-bonus rejection, white active/locked rendering and exact symbol/numeric count after each transition.

## R13–R15 — Configuration, persistence and failures

[CONFIGURATION](spec/CONFIGURATION.md) is the canonical owner of the six settings, defaults, live/captured behavior, serialization, per-key precedence, invalid inputs and independent best-effort browser effects. C-CONFIG, C-CONTROLLER, C-PANEL and C-PREFERENCES implement those contracts. No other source defines an alternative settings format.

## R16 — Presentation and usability

Display a complete board, score, lines, level, session status, visible controls and key legend. Side panels expose Hold, optional next preview, bonus indicator and the six labeled HTML controls. Each toggle has readable state; pending lock policies are visibly distinguished from active piece policies. Hide disabled next preview without consuming the source. Board cells remain square and the complete board and controls remain accessible at 1024×768 and 1440×900 desktop viewports; scrolling to lower controls is permitted without clipping the board. Controls support native Tab/keyboard interaction and readable text contrast; do not claim a fully nonvisual playable board. View snapshots never lag bonus/settings state by more than the next animation frame.

Acceptance: start/play/pause/game-over/restart flows, Canvas geometry/color/ghost observations, keyboard/form coexistence, both viewport sizes and visible selected/pending state.

## R17 — Verification qualities

Mechanics must be deterministic under supplied random/time inputs, independent of rendering partition. Browser acceptance uses the actual built application and isolated managed Chromium headless shell as primary plus managed Firefox as secondary, following [browser policy](../../BROWSER_DEV_POLICY.md). Both browsers must complete applicable lifecycle/input/control/persistence/rendering checks. Browser installation, launch alone, or synthetic HTML is insufficient application evidence. Inspect effective runner/browser versions and at least two isolated context/page cycles and teardown. A missing required browser blocks its affected acceptance claim; preserve independently verified evidence. No browser is required for document-only work.

## R18 — Distribution and releases

Build a static site with no runtime backend. Package metadata, displayed version and release tag must agree. Release the classic baseline as 0.1.0; capability releases increment minor versions until the full verified target is 1.0.0, with patch increases for fixes. After each completed verified integrated implementation phase, publish its version tag and GitHub release from an eligible default-branch release commit with matching finalized release notes. Root RELEASE-NOTES.md describes delivered scope, setup/play instructions, checks and material limitations. No documentation-only release is required. Runtime assets exclude development credentials, browser binaries and test state. The application must work when served at a non-root static path with its own asset URLs.

Acceptance: a clean locked-dependency build, served built assets at a subpath, matching version identifiers, release-source/tag/body readback and package inspection without secrets or generated browser state.

## End-to-end acceptance routes

| Route | Observable result / contract coverage |
| --- | --- |
| Classic play | Start, move, rotate, gravity, clear, score/level, pause/resume, top-out and restart; R01–R05, R16–R18 |
| Standard assistance | Seven-bag trace, empty/populated Hold, SRS kicks, ghost/drop identity, both locking policies; R06–R10 |
| Penetrator | Earn bonus, activate through a barrier, see matching deepest ghost, reject Hold, safely lock white cells, earn again/cap; R08–R12 |
| Session settings | Exercise all six controls including pending lock-policy feedback without mutating board/Hold; R13, R16 |
| Restore/share/failure | Mixed URL/cookie/default resolution, explicit-default cookie writes, omitted keys, invalid/duplicate inputs, per-key changes, reload and unavailable storage/history; R14–R15 |

Every route preserves previously delivered behavior. Detailed checks may use deterministic domain fixtures and controlled adapter failures, while real browser routes establish integration and visible output; no single layer substitutes for the others.
