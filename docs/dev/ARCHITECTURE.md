# Architecture

The [project brief](PROJECT.md) defines a static desktop browser game. The system separates deterministic game rules from browser input, animation, rendering and preference storage. No server owns gameplay or configuration.

## Blocks and dependency direction

| Block | Responsibility | Dependencies |
| --- | --- | --- |
| Game domain | Board and pieces, legal actions, gravity/locking, scoring, randomizer, Hold, landing projection and penetrator lifecycle | Plain data, elapsed gameplay time and injected random source; no browser APIs |
| Application controller | Own the session, order commands, advance domain time, coordinate settings and expose snapshots | Domain and narrow clock/input/preference ports |
| Presentation | Canvas board and ghost; HTML panels, controls and status; keyboard input adapter | Read-only session/settings snapshots and controller commands |
| Browser adapters | Animation scheduling, visibility/focus, cookie and URL access | Browser APIs; preference codecs from application boundary |
| Composition root | Construct dependencies and release their resources | Concrete blocks; no gameplay rules |

The controller calls the domain; the view consumes its snapshots and sends commands back through the controller. Domain transitions never call the view or a browser adapter. Browser effects terminate at adapters, which report failure without mutating game rules. Shared types flow toward the domain rather than introducing a dependency cycle. A small reducer-style transition module is sufficient; no event bus, dependency injection framework or class hierarchy is required.

## State and authoritative calculations

The controller owns one current session value and one selected-settings value. The domain owns the settled grid, active piece, held type, preview/randomizer state, score/line/level totals, bonus count and grounding timer. Consumers receive immutable snapshots; they do not patch a board or timer directly. A transition atomically returns the next state and presentation-relevant results such as a line clear or rejected action.

Geometry and placement queries are shared by movement, locking and projection. Normal landing respects intervening settled cells. Penetrator landing searches legal non-overlapping placements below the current pose despite transit overlap. Ghost and hard drop consume the same landing result, avoiding two competing interpretations. A placement admitted for transit is never sufficient to authorize locking. Line removal and bonus accounting occur only after a validated lock.

The domain consumes explicit elapsed gameplay milliseconds and an injected random source, enabling exact timer boundaries and reproducible piece sequences. The browser clock uses monotonic time. Paused/background time does not enter domain elapsed time; resuming starts a new clock anchor. User commands and elapsed time are serialized, so a lock cannot race with Hold, a setting change or penetrator activation.

## Configuration boundary

The configuration model owns typed values, defaults and validation. Restore resolves each setting independently and distinguishes absent, invalid and explicit valid URL inputs. Cookie and URL adapters supply raw values and accept writes; they do not select precedence. Selected lock policies are copied into each newly spawned active piece. Other switches affect the current view/action eligibility immediately.

Preference writes are best effort and independent. A failed cookie operation does not prevent updating the URL or session value; a failed URL operation does not prevent updating the cookie or session value. This boundary prevents browser restrictions from breaking play. Persistence stores preferences only, never domain session state.

## Browser interaction and resources

Use Canvas for frequent board drawing and HTML for labels, buttons and configuration controls. This preserves straightforward form interaction while keeping the board renderer independent of DOM layout. A DOM-cell board is viable but requires managing many elements; a UI framework adds lifecycle/state machinery without a requirement that justifies it. Canvas is the selected approach from the brief.

One controller lifecycle owns one animation callback chain and its event listeners. Restart replaces session state, not adapters; teardown cancels the chain and removes listeners. Keyboard handling suppresses browser scrolling only for consumed game keys and ignores editable/form targets. Visibility loss and window blur suspend play and clear pressed keys. The player explicitly resumes after returning.

## System invariants and verification seams

- Settled cells remain in bounds and never overlap a locked piece before placement validation.
- Regular piece colors exclude white; penetrator cells are white and retain a contrasting outline against the board.
- One active piece has at most one penetrator activation and one Hold use between locks; Hold is unavailable during penetration.
- Configuration changes cannot erase a held piece, mutate the settled board or restart a running lock deadline.
- Domain tests use synthetic time/randomness; adapter tests use controlled ports; browser checks exercise the actual application and Canvas output in isolated Chromium and Firefox.

Detailed logical owners are in [DECOMPOSITION](DECOMPOSITION.md); observable contracts belong in SPEC. The design intentionally uses no additional architecture children because these blocks and their collaborations fit in one coherent root.
