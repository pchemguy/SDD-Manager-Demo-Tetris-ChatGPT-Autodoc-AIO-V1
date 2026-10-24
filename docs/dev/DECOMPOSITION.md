# Component decomposition

The [architecture](ARCHITECTURE.md) establishes the domain/controller/view/adapter boundary. This component model defines logical responsibility and verification seams; it does not require one class or file per row. The composition root is the only consumer allowed to construct concrete browser dependencies.

## Logical owners

| ID / component | Owns and provides | Requires / excludes | Verification seam |
| --- | --- | --- | --- |
| C-GEOMETRY | Tetromino cell masks, orientations, kick candidates and coordinate transforms | Plain poses and board queries; no timers or session mutation | Enumerate shapes, rotations, wall/floor boundaries and candidate order |
| C-BOARD | Settled grid, placement legality, atomic placement and complete-row removal | Cell coordinates; never owns active-piece transit | Synthetic grids with overlap/top-out/adjacent-row cases |
| C-LANDING | Normal landing and penetrator lowest legal landing; grounding queries | Geometry and read-only board; never writes cells or calculates scores | Compare ghost/drop results over the same supplied poses and boards |
| C-PIECES | Piece source and upcoming queue, seven-bag shuffling, held type and per-piece Hold eligibility | Injected random values and spawn factory; never owns browser input | Seeded sequences and controlled next/hold traces |
| C-GAME | Session transition state, active piece and captured policies, lock deadline, score/level/lines, bonuses and status | Board/geometry/landing/piece source; explicit commands and elapsed time | State transitions with synthetic time, rejected-action and lock/clear traces |
| C-CONFIG | Typed settings, defaults, raw value codec, per-key restore resolution and selected/pending policy view | Raw strings and preference ports; never writes domain board or active policy snapshot | Table-driven absent/valid/invalid and mixed-source resolution |
| C-CONTROLLER | Session/settings ownership, command ordering, clock anchoring, restart/pause and effect orchestration | C-GAME, C-CONFIG and injected adapter ports; never owns geometry rules | Fake clock and ports, snapshots after ordered commands and failed effects |
| C-INPUT | Keyboard-to-command mapping, repeat scheduling and focus/blur filtering | Controller command sink and clock; no board access | Input events with editable targets, key transitions and release/blur traces |
| C-RENDER | Canvas board/active/ghost drawing from snapshots | Read-only geometry/snapshot; no domain mutation | Pixel/shape checks with known grids and white/nonwhite palettes |
| C-PANEL | HTML score/status, Hold/next/bonus display, controls and pending-policy labels | Read-only snapshots and controller commands; no independent settings copy | Real form interaction, labels, values, focus and bonus symbol count |
| C-PREFERENCES | Browser cookie/URL read and write adapters with classified success/failure | Browser APIs and serialized strings; no precedence or gameplay policy | Port-level malformed/unavailable storage and URL-preservation checks |

## Shared data boundaries

Use a bounded piece type and orientation; a pose consists of type, orientation and integer origin coordinates. Cell lists are derived from geometry. Settled board cells contain regular piece identity or a penetrator marker, allowing white rendering without changing collision semantics. Domain snapshots expose grid, active pose/flags, optional landing, held/next types, totals, bonuses, status and captured lock policies.

Landing queries return either a pose/distance or no legal result. A penetrator's transit position may overlap settled cells; a normal position may not. C-GAME validates the appropriate query before spending a bonus or accepting a movement. No-result is an ordinary rejected action, not an exception that unwinds the controller. Locking requires a separate full visible-board placement check even after a landing query.

C-CONFIG distinguishes selected values from the current piece's captured general/hard-drop policies. A settings snapshot also exposes whether those policies are pending for the next spawn. That snapshot is sufficient for the panel; it need not inspect a lock deadline.

## Collaboration and transaction boundaries

1. C-INPUT or C-PANEL submits one intent to C-CONTROLLER. The controller first advances elapsed active time to the intent timestamp, then applies the intent to the resulting current session. A command cannot act on a piece whose deadline already expired.
2. C-GAME obtains geometry/landing and piece-source results, builds a next session, and performs at most one terminal session transition at a time. Placement, line clear, score, bonus award, next spawn and Hold-eligibility reset form one atomic lock transition.
3. C-CONTROLLER publishes a consistent snapshot. C-RENDER and C-PANEL consume it; rejected commands leave relevant state unchanged.
4. A control change updates selected configuration once. C-CONFIG serializes that key; the controller independently requests cookie and URL effects through C-PREFERENCES. Failure changes persistence feedback, not the selected session value.

The controller supplies elapsed active time between transitions, without counting suspended time. C-GAME owns processing gravity/deadline events in temporal order, including exact-boundary locking. The input adapter owns repeat timing but routes every repeated intent through the same ordering rule.

## Lifecycle and failure ownership

New game clears board/totals/Hold/bonuses and obtains a fresh randomizer; selected configuration remains. New active-piece construction captures selected lock policies. Hold replacement is a new spawn with fresh timing and policies, while retaining the once-per-lock Hold restriction. Activating penetration clears any obsolete normal grounding deadline and evaluates penetrator grounding against its legal landing.

The controller owns mount/teardown and the sole animation subscription. C-INPUT clears held keys on pause, blur, visibility loss and teardown. C-PREFERENCES returns read/write results instead of throwing into the render loop; unavailable or malformed individual cookie/URL values fall through under C-CONFIG. Missing Canvas context is an initialization failure with a visible message and no partial game loop.

## Boundary assessment

C-GAME coordinates closely coupled game rules rather than exposing independently mutable subsystems. Geometry/board/landing queries remain pure and reusable; C-PIECES can be tested without rendering. Browser adapters depend on controller contracts and never reverse the domain dependency. This keeps deterministic tests representative without mocking every internal function. Exact input, timing, configuration and failure contracts belong in SPEC; physical allocation belongs in layout.
