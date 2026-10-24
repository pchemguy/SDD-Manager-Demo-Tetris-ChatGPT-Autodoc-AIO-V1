# Browser Tetris

## Purpose and users

Create a single-player browser Tetris game for desktop keyboard players. Deliver a simple, complete playable game first, then useful bounded increments that preserve earlier gameplay. The complete target combines familiar tetromino play with configurable assistance, a penetrator bonus mechanic and persistent preferences.

## Intended experience

The classic baseline includes a visible board, seven tetromino shapes, lateral movement, rotation, gravity, collision, locking, line clearing, score, increasing level/speed, game over and restart. Pause/resume prevents background time from advancing play. Later increments add Hold, ghost projection, seven-bag randomization, SRS wall kicks and hard drop; a side panel displays score, level, lines, optional next preview, Hold and available bonuses.

The default general lock policy waits one full current gravity interval after grounding. Hard drop uses that policy unless immediate locking is selected. A selectable alternative locks on a failed downward movement attempt.

Ghost visibility supports always hidden, dynamic visibility and always visible. Dynamic mode suppresses a landing projection in the upper half of the board. A penetrator's ghost and hard drop share the same lowest legal landing calculation, including passage through occupied cells.

Clearing four lines simultaneously earns one penetrator activation, capped at three stored bonuses. A hotkey spends one bonus to turn the current piece white until it locks. It passes through occupied cells without erasing them, but may lock only wholly inside the board and without overlap. Regular pieces never use white. The side panel shows one symbol per available bonus and updates on earning or spending.

## Configuration and persistence

Provide six side-panel controls: three-state ghost visibility; hard-drop enablement; next-preview enablement; follow-general versus immediate hard-drop locking; failed-down versus full-interval general locking; and Hold enablement. Session-only controls may precede persistence.

Resolve each setting independently from a valid explicit URL query value, then its valid cookie, then the default. Persist valid explicit URL values to cookies even when they equal defaults. Omitted parameters preserve cookie preferences. User changes update the corresponding cookie and URL parameter without dropping unrelated query parameters or the fragment. Invalid inputs must not crash the game or silently overwrite a valid saved preference. Preference persistence excludes scores, game progress and held pieces.

## Proposed consequential choices

The following are recommendations for approval, rather than requirements explicitly supplied by the project description. Their precise behavioral contracts belong in the specification.

| Concern | Recommended choice and reason |
| --- | --- |
| Platform and presentation | Static client application using TypeScript, Canvas for the board and semantic HTML controls. No backend or UI framework is needed. This keeps the game small and permits deterministic logic tests. |
| Board and ghost midline | Use 10 columns and 20 visible rows. Show a dynamic ghost only when every projected cell is in the lower ten rows; a straddling ghost stays hidden. This gives an unambiguous threshold. |
| Timing and reset | Capture the gravity interval when grounding begins. Grounded movement/rotation does not reset that deadline. Becoming airborne cancels it; later grounding starts a new interval. Pause and background suspension freeze elapsed gameplay time. Hard drop starts a fresh interval only when it newly grounds the piece, preserving an already running grounded deadline. |
| Penetrator and Hold | Disable Hold while a penetrator is active. Activation is one-way for the current piece, cannot spend again while active, and ends on lock, restart or game over. Stored bonuses start at zero on restart. This avoids transferring the ability through Hold. |
| Penetrator safety | Require a legal landing at or below the current position before activation; reject unavailable/invalid activation without spending a bonus. Permit transit overlap, never locking overlap. Grounding and failed-down locking use the lowest legal reachable landing, rather than an intermediate occupied cell. Reject a lateral move or rotation that would leave no legal landing below. |
| Live settings | Ghost, preview, hard-drop and Hold switches take effect immediately without deleting a held piece. Lock-policy changes apply to the next spawned piece, avoiding retroactive timer changes; indicate this in the controls. |
| Invalid configuration | Invalid URL values fall through to a valid cookie/default for that setting and do not overwrite its cookie. Invalid cookies fall through to defaults. Repeated recognized parameters are treated as invalid to avoid ambiguous precedence. Disabled cookies leave URL and session controls usable. |
| Defaults | Dynamic ghost, hard drop on, next preview on, full-interval general locking, hard drop follows general locking, Hold on. Newly introduced controls use these defaults. |
| Input | Arrow keys move/soft-drop; Up/X rotate clockwise, Z counterclockwise, Space hard-drops, C holds, P pauses, B activates penetrator. Ignore gameplay hotkeys while a form control is focused. Bindings can be documented without adding remapping UI. |
| Verification | Headless Chromium is primary and managed Firefox secondary, in dedicated isolated environments under BROWSER_DEV_POLICY.md. Document preparation needs no browser provisioning. Required application verification remains subject to actual runner and browser readiness. |

## Scope limits and delivery direction

Proposed exclusions are multiplayer, accounts, remote leaderboards, mobile touch controls, audio, input remapping and saved in-progress games. Keep desktop keyboard operation and readable controls usable at common viewport sizes. Detailed scoring, speed, spawn/top-out, SRS and input-repeat contracts are delegated to specification within this scope.

Delivery should progress from the classic baseline to standard play enhancements, then conditional ghost and penetrator behavior, session configuration and persistent preferences. PLAN determines the final bounded phase and milestone allocation; this brief does not impose one phase per feature. Each completed, verified and integrated implementation phase receives a tagged GitHub release. Start the playable baseline at 0.1.0, increase the minor version for subsequent capability phases and use patch versions for fixes; reserve 1.0.0 for the verified complete target. Documentation alone is not a game release.

## Success

A player can start, play, lose and restart the baseline; use each later capability through keyboard and visible controls; predict penetrator landing from its ghost; change preferences without corrupting active play; and recover settings consistently on reload and shared URLs. Verification must cover deterministic mechanics and the actual browser interaction/rendering path. Intended functionality is not evidence that it has been implemented.
