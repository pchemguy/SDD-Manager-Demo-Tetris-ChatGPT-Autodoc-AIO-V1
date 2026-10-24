# Delivery plan

Deliver the complete [SPEC](SPEC.md) through five bounded, independently useful releases. Phase 1 is a playable classic game; subsequent phases preserve it while adding standard assistance, penetrator play, session controls and durable preferences. Browser provisioning is an early risk check, not a product increment. No phase requires all later requirements to claim its own scoped completion.

## Phase and release map

| Phase | Name | Version | Useful outcome | Included scope / explicit deferrals |
| --- | --- | --- | --- | --- |
| 1 | Classic baseline | 0.1.0 | Start, play, clear/score, level up, pause, lose and restart | R01–R05 normal pieces, full-interval default, rotation offset (0,0), independent-uniform source, R16–R18 applicable baseline qualities. Defer Hold, preview, bags, kicks, ghost, hard drop, penetration and six settings. |
| 2 | Standard assistance | 0.2.0 | Predict, rotate, hold and place pieces with modern assistance | R06–R10: seven-bag, preview, full SRS, Hold, always-visible legal ghost and hard drop following full-interval locking. Defer conditional visibility, bonuses/penetration, control panel and selectable alternate policies. |
| 3 | Penetrator play | 0.3.0 | Earn/spend bonuses and traverse stacks with matching ghost/drop | R08 dynamic default, R11–R12, white pieces, Hold restriction and safe deepest landing. Defer player settings and persistence; hard drop retains full-interval default. |
| 4 | Session configuration | 0.4.0 | Customize all six settings safely during a session | R13, alternate general/immediate drop policies in R04/R09, all visibility modes, pending-policy feedback and full interaction matrix. Settings reset to defaults on reload; defer R14–R15 browser persistence. |
| 5 | Persistent preferences | 1.0.0 | Restore/share preferences with reliable precedence and graceful failures | R14–R15, all six exact codecs, cookie/URL restore/writes, failure feedback and complete-target regression, distribution and release evidence. No remaining accepted scope is deferred. |

Phases depend on their immediate predecessor's verified integrated release baseline. Within a phase, milestones are ordered by the dependencies below; implementation work can use independent unit checks but integrated milestone demonstrations happen promptly. Version 1.0.0 denotes the full target, rather than expanding Phase 1 to the entire feature list. Patch releases may repair already delivered scope without consuming the next phase's capability version.

## Phase 1 — Classic baseline

### Milestone 1.1 — Falling-piece play path

Establish a locked TypeScript/static-build/unit-test toolchain and isolated matching Chromium/Firefox runner readiness before dependent browser work. Build geometry, settled board queries, normal-piece movement/rotation, gravity/full-interval lock transitions and a minimal Canvas/controller/input loop. Demonstrate a piece moving, falling and locking in the actual application. This is an enabling play path, not a completed classic release. No modern controls/source abstractions need visible product UI yet.

Exit: deterministic masks/collisions/timer checks and visible browser movement/locking; bounded runner lifecycle/provisioning evidence for each browser; review the integrated core, repair blockers and retain a milestone report. The demonstration tests whether movement/readability and the chosen browser facility justify continuation or need human-directed amendment.

### Milestone 1.2 — Complete classic game loop

Depends on 1.1. Add atomic line clearing, scoring/level speed, game-over/top-out, ready/start/restart, pause/background suspension and visible score/status/lifecycle controls. Complete focus filtering and keyboard repeat behavior. Demonstrate a playable loss-and-restart loop and a clear/level fixture. This is the earliest complete classic use path; browser input and model tests jointly establish the outcome.

Exit: R01–R05 baseline acceptance, no suspended-time jumps or duplicate loops, keyboard/form coexistence and whole-capability review/report. Exclude enhancement UI rather than displaying nonworking controls.

### Milestone 1.3 — Reproducible baseline distribution

Depends on 1.2. Complete actual-browser lifecycle/rendering acceptance, locked-dependency CI/build checks, static subpath serving, clear play/setup instructions, version metadata and baseline release notes. Keep owned browser artifacts/test state outside distribution and Git.

Exit: repeated isolated page/context teardown in Chromium and Firefox, screenshots or shape/pixel observations for the actual game, clean reproducible build and served built output, matching 0.1.0 identifiers, report and clear limits. A successful environment probe alone is not application acceptance.

### Milestone 1.4 — Phase review

One phase review/testing/report task after 1.1–1.3 complete and their tracking milestones close. Recheck integrated classic exits, cross-milestone input/timing/build regressions and required evidence. Preserve TODO dispositions. Verify eligible phase integration and merged-state checks before publishing 0.1.0. No enhancement is required at this boundary.

## Phase 2 — Standard assistance

### Milestone 2.1 — Predictable source and rotation

Depends on Phase 1 integration. Introduce seven-bag generation, stable one-piece preview and complete SRS kick tables. Keep source lookahead independent of rendering. Demonstrate seeded bag boundaries and wall/floor kicks in the running game. Exit: source/kick fixtures, visible next preview, prior collision/timing regressions and whole-milestone review/report.

### Milestone 2.2 — Hold workflow

Depends on 2.1. Add empty/populated Hold, spawn resets/collision, once-per-lock eligibility and visible Hold state. Demonstrate storing/retrieving a piece without corrupting source order. Exit: normal/failed swaps, repeated-use rejection, pause/restart integration and whole-milestone review/report. No player Hold toggle is needed yet.

### Milestone 2.3 — Landing assistance

Depends on 2.2. Establish the shared normal landing query, visible ghost and hard drop with distance points and full-interval follow locking. Demonstrate ghost/drop equality at floor and stacks; show zero-distance repeats do not reset deadlines. Exit: R06–R10 applicable scope and previous baseline browser regressions; current 0.2.0 notes/metadata, whole-milestone review/report. Always-show is this release's fixed ghost behavior; dynamic mode is scheduled for Phase 3.

### Milestone 2.4 — Phase review

One review/testing/report task after delivery milestones close. Verify modern assistance in both required browsers, source/Hold/ghost/drop interactions and preserved classic behavior. Integrate, check the merged application and publish 0.2.0 at the eligible release boundary.

## Phase 3 — Penetrator play

### Milestone 3.1 — Safe penetrator traversal

Depends on Phase 2 integration. Implement deepest legal landing through occupied cells, no-result rejection, transit/grounding/activation safety, disabled Hold, white active/locked rendering and shared ghost/drop behavior. Domain tests may supply a bonus fixture; the milestone must not advertise naturally earned bonuses until 3.2. Demonstrate the barrier/cavity scenario and rejected unsafe action. Exit: no overlap writes or accidental barrier destruction; correct normal→penetrator timer evaluation; whole-milestone review/report.

### Milestone 3.2 — Earned bonuses and conditional projection

Depends on 3.1. Integrate four-line bonus award/cap/spend, exact symbol/numeric indicator and dynamic ghost visibility including straddling and row-ten threshold. Demonstrate the complete earn/activate/drop/lock sequence through production rules, using deterministic board fixtures only to make the scenario reproducible. Exit: R08/R11–R12 interactions and prior assistance regressions, current 0.3.0 notes/metadata and whole-milestone review/report. This demonstration supports human judgment of the unusual landing/visibility behavior; a requested change returns to its owning contract rather than being silently improvised.

### Milestone 3.3 — Phase review

One review/testing/report task after both delivery milestones close. Verify bonus accounting, white rendering, Hold restriction, deepest ghost/drop equality, top/floor safety and timing regressions. Integrate, check and publish 0.3.0. Session configuration/persistence remain outside this phase.

## Phase 4 — Session configuration

### Milestone 4.1 — Immediate display and action controls

Depends on Phase 3 integration. Establish typed selected configuration and session defaults. Expose ghost mode, hard-drop enablement, next-preview enablement and Hold enablement. Preserve held/source/session state while controls update immediate behavior. Exit: four live controls with native focus semantics, hide/dynamic/show thresholds, disabled-action rejection, no queue consumption by rendering, and whole-milestone review/report. Two lock-policy controls follow in 4.2; no persistence is claimed.

### Milestone 4.2 — Configurable lock policies

Depends on 4.1. Add failed-down general locking and immediate hard-drop locking, selected-versus-captured policy state, the two remaining controls and truthful next-piece feedback. Demonstrate that changing a policy cannot retroactively extend or force the current lock, including Hold replacement. Exit: six-control session acceptance, general×hard-drop×normal/penetrator combinations, pause/form input and prior routes, current 0.4.0 notes/metadata and whole-milestone review/report. Help text explicitly states settings are session-only in this release.

### Milestone 4.3 — Phase review

One review/testing/report task after both delivery milestones close. Verify the full session configuration interaction matrix and preserved play in Chromium/Firefox. Integrate, verify and publish 0.4.0 without claiming reload persistence.

## Phase 5 — Persistent preferences

### Milestone 5.1 — Correct restoration

Depends on Phase 4 integration. Add exact codecs/raw-state distinctions, cookie/URL readers and independent per-key restoration before first spawn. Persist valid explicit URL values to cookies including defaults, preserve omitted-key cookies and handle duplicates/invalid values without overwriting valid preferences. Exit: per-key and mixed-source resolution matrix, visible restored controls/current policies, explicit-default reload behavior and whole-milestone review/report. Later user-change write paths are 5.2's outcome.

### Milestone 5.2 — Durable changes and resilient sharing

Depends on 5.1. Connect user changes to independent cookie/history writes, canonicalize only the changed URL key and preserve unrelated query/fragment data. Add storage/history failure detection and nonblocking truthful feedback. Verify reload, shared URLs, disabled cookies and settings retained across restart. Complete full-target browser/build/distribution evidence, 1.0.0 notes/setup instructions and whole-milestone review/report.

### Milestone 5.3 — Phase review

One review/testing/report task after both delivery milestones close. Verify the complete target and preserved previous routes, aggregate all unresolved/deferred milestone and phase TODOs into the final implementation report, and check source/package/version/release-note consistency. Integrate, run merged-state checks and publish 1.0.0 only with all required full-target evidence.

## Cross-cutting evidence and release gates

Every delivery milestone ends with whole-capability code review, applicable tests/regressions, blocker repairs, a committed milestone report and refreshed HANDOFF. TASKS assigns this to its final implementation task unless the lifecycle threshold requires a standalone review task. Special phase review milestones always contain exactly one phase review/testing/report task and begin after all delivery milestone closures. Reports use the phase/milestone paths defined in layout and include TODO=None when empty.

The phase review task prepares version/build/release-note evidence and phase integration readiness. The coordinator then performs eligible explicit integration, verifies the merged state and publishes it; a tagged release and provider readback follow from that verified default-branch boundary. Notes are not published before the corresponding implementation phase passes. No new standalone release task is needed solely to close a phase. Task execution and hosted tracking follow the governing SDD workflow, with one initial owning completion per task and published evidence.

Domain checks are deterministic; controller/adapter checks exercise time, failure and source ordering; browser routes prove real input/rendering/persistence. Browser readiness must be established separately for Chromium and Firefox under BROWSER_DEV_POLICY.md before dependent work. An unavailable required facility blocks affected acceptance; investigate compliant remedies and retain partial evidence without changing browser scope implicitly. No optimistic environment claim substitutes for a required check.

## Coverage and principal risks

| Contract route | First delivery | Extended checks |
| --- | --- | --- |
| R01–R05 | 1.1–1.3 | SRS/Hold/source in Phase 2; penetrator contact in Phase 3; policy combinations in Phase 4 |
| R06–R10 | 2.1–2.3 | Dynamic/penetrator projection in Phase 3; all modes/policies in Phase 4 |
| R11–R12 | 3.1–3.2 | Selected controls/policies in Phase 4; restored selections in Phase 5 |
| R13 | 4.1–4.2 | Restore/write/error paths in Phase 5 |
| R14–R15 | 5.1–5.2 | Reload, explicit-default/omitted inputs and independent-failure end-to-end routes |
| R16–R18 | Every phase's scoped UI, browser/build/version gates | Complete target and final report at 5.3 |

Main risks are browser provisioning/session restrictions, differences between penetrator transit and lock legality, timer ordering around policy changes, and silent cookie rejection. Address them at the early environment milestone and the owning capability milestones. Full game persistence, mobile touch, multiplayer and a backend remain out of scope. No unresolved product decision is required to derive layout and tasks.
