# Implementation tasks

[PLAN](PLAN.md) owns delivery outcomes and exits; [SPEC](SPEC.md) with its two children owns behavior; [layout](layout.md) owns source/test/report placement. All items below are planned and unchecked. This list is the sole executable implementation owner.

## Execution and completion rules

Each task includes its focused checks and relevant documentation, fixes blockers and preserves earlier delivered behavior. Complete one owning task with result/evidence and its initial checkbox in one commit, publish/read back, then reconcile its issue. Required browser work depends on verified isolated readiness under BROWSER_DEV_POLICY.md; do not waive blocked browser evidence or count a synthetic probe as application acceptance.

Each phase starts only after its predecessor has completed review/closure, explicit integration, merged-state verification, publication and requested release lifecycle. Activate only the eligible phase tracking. Review tasks count as executable tasks. Every milestone boundary includes whole-capability code review, required checks/repairs, a committed report with TODO dispositions and HANDOFF; phase reviews include cross-milestone regressions. Release publication follows verified default-branch integration under coordinator ownership, rather than tagging a task/phase branch.

Milestone 1.1 has four delivery tasks and a standalone review task. All other delivery milestones have three delivery tasks; their final implementation task owns the folded milestone review/report/HANDOFF. Each phase review milestone remains exactly one task.

## Phase 1 — Classic baseline

- [ ] Phase 1 — Classic baseline
    - [ ] Milestone 1.1 — Falling-piece play path
        - [ ] T-001 — Establish locked toolchain and isolated browser readiness
            Scope: package.json/package-lock.json, TypeScript/Vite/Vitest/Playwright configs, .gitignore; dedicated owned browser location.
            Depends on: None; setup task for the authorized Phase 1 baseline.
            Outcome: Create static entry/build/type/unit/e2e scripts and a reproducible locked toolchain. Provision matching isolated Chromium headless shell and managed Firefox; diagnose each independently under browser policy before dependent browser work. Record actual host/versions/locations and validated commands; preserve unrelated toolchains.
            Contracts: R17–R18.
            Evidence: Clean locked install, type/unit/build smoke, separate finite browser launch/context/page/interaction/render/two-cycle/teardown probes. Synthetic probe is environment evidence only; missing required readiness blocks dependent application checks. No production capability claim.
        - [ ] T-002 — Implement piece geometry and settled-board kernels
            Scope: src/domain/types.ts, geometry.ts, board.ts; tests/unit/domain/geometry.test.ts and board.test.ts.
            Depends on: T-001.
            Outcome: Implement seven spawn masks/orientation transforms, occupied-cell bounds, immutable settled grid queries and placement validation. Baseline rotation uses zero-offset only; retain a seam for kick candidates without implementing modern assistance.
            Contracts: R01, R10 baseline.
            Evidence: Enumerate four cells per mask, four rotations, walls/floor/hidden-row boundaries, overlap rejection, visible spawn collision and unchanged input grids.
        - [ ] T-003 — Implement normal movement gravity and delayed locking
            Scope: src/domain/game.ts, pieces.ts; tests/unit/domain/game.test.ts and pieces.test.ts.
            Depends on: T-002.
            Outcome: Implement active pose, independent-uniform baseline source, lateral/soft movement, temporal event processing, full-interval grounding/deadline and validated normal placement. Keep score/clear/lifecycle extensions localized for 1.2.
            Contracts: R01–R04 baseline.
            Evidence: Seeded normal piece traces, successful/failed moves, deadline −1/exact/+1, grounded no-reset, escape/reground, equal elapsed time under different partitions and legal placement only.
        - [ ] T-004 — Connect the falling-piece browser play path
            Scope: src/app/controller.ts, src/browser/clock.ts and keyboard.ts, src/ui/board.ts, src/main.ts, index.html, styles.css; focused tests/e2e.
            Depends on: T-003.
            Outcome: Compose the real controller/time/input/Canvas path. Render the grid and regular palette, deliver movement/rotation/down keys and show pieces falling/locking. Keep one loop/listener owner; omit unfinished enhancement controls.
            Contracts: R01–R04, R16–R17 baseline.
            Evidence: Actual application movement/locking/rendering in both ready browsers; fake-clock ordering, key-repeat suppression and teardown checks. Demonstrate readable controllable falling-piece play; retain prototype feedback without claiming complete classic scoring.
        - [ ] T-005 — Review test and report the falling-piece play path
            Scope: Integrated 1.1 source/tests; docs/dev/reports/phases/1/1.1.md and HANDOFF.md.
            Depends on: T-001, T-002, T-003, T-004 and accepted completion of all 1.1 delivery work.
            Outcome: Perform whole-capability code review and applicable core/browser checks, repair blockers, assess milestone demo/usefulness and retain review/check evidence with TODO dispositions.
            Contracts: R01–R04, R16–R17 baseline.
            Evidence: All four delivery tasks accepted/published, actual play-path checks pass, report and HANDOFF in this task completion commit; close/read back milestone only after all constituent issues close.
            Standalone milestone review: review the whole 1.1 capability, execute required checks/regressions, repair blockers, retain TODO dispositions and demonstrate its PLAN outcome before this task completes.
            Report: docs/dev/reports/phases/1/1.1.md; refresh docs/dev/HANDOFF.md in the same completion commit. Close/read back the milestone after all task issue closures.
    - [ ] Milestone 1.2 — Complete classic game loop
        - [ ] T-006 — Implement atomic clearing scoring and level progression
            Scope: src/domain/board.ts and game.ts; focused unit fixtures.
            Depends on: T-005.
            Outcome: Add simultaneous row removal/compaction, line points using pre-clear level, soft-drop points, line/level totals and level-dependent gravity. Preserve legal-lock/top-out atomicity.
            Contracts: R01, R04–R05.
            Evidence: 1/2/3/4 and adjacent/nonadjacent clears, empty top fill, 9→10-line scoring, gravity floor and no partial top-out mutation.
        - [ ] T-007 — Implement session lifecycle and suspension
            Scope: src/domain/game.ts, src/app/controller.ts, src/browser/clock.ts and keyboard.ts; unit/integration checks.
            Depends on: T-006.
            Outcome: Add ready/start, running/paused/game-over, top-out, restart, explicit resume after blur/hidden and fresh clock anchors. Clear repeat state at lifecycle/spawn boundaries; retain selected defaults.
            Contracts: R01–R03, R05.
            Evidence: Pause for long synthetic time without a jump, background/blur traces, top-out/no replacement consumption, repeated restarts and no duplicate chains/listeners.
        - [ ] T-008 — Complete classic panels and keyboard interaction
            Scope: src/ui/panel.ts and styles.css, src/browser/keyboard.ts, controller/main integration; tests/e2e, README baseline instructions.
            Depends on: T-006, T-007 and the preceding milestone/phase prerequisite.
            Outcome: Display score/lines/level/status, lifecycle buttons and key legend. Complete repeat arbitration, native form focus and viewport behavior. Integrate play→clear→lose→restart demonstration and record usable baseline instructions.
            Contracts: R02–R05, R16–R17.
            Evidence: Real native keys/buttons/form focus and both viewports, score/level fixture, full classic lifecycle and preserved timer behavior.
            Folded milestone exit (part of this implementation task): review the whole 1.2 capability, execute required checks/regressions, repair blockers, retain TODO dispositions and demonstrate its PLAN outcome before this task completes.
            Report: docs/dev/reports/phases/1/1.2.md; refresh docs/dev/HANDOFF.md in the same completion commit. Close/read back the milestone after all task issue closures.
    - [ ] Milestone 1.3 — Reproducible baseline distribution
        - [ ] T-009 — Verify classic application browser acceptance
            Scope: tests/e2e and tests/fixtures; isolated runner projects and owned screenshot outputs.
            Depends on: T-008.
            Outcome: Implement actual-application classic lifecycle/input/rendering checks in both browsers, including two isolated context/page cycles and teardown. Use test-only fixtures for hard-to-reach rows while retaining built-application routes.
            Contracts: R01–R05, R16–R17.
            Evidence: Inspect effective versions/modes, board shape/colors, actual input and screenshots/pixel observations; finite whole-process limits and no leaked owned sessions. Synthetic/harness versus built-app evidence is distinguished.
        - [ ] T-010 — Make static build and CI reproducible
            Scope: vite.config.ts, package scripts/lockfile, .github/workflows/ci.yml; dist inspection/subpath test.
            Depends on: T-009.
            Outcome: Establish clean install/type/unit/build/browser checks and dedicated browser setup in CI. Verify served built assets at a non-root path and exclude fixtures, credentials, binaries and test state.
            Contracts: R17–R18.
            Evidence: Clean build and subpath smoke, ignored-artifact/package inspection, CI command/run evidence where available. A prepared workflow alone is not a passing hosted run.
        - [ ] T-011 — Prepare playable baseline instructions and version metadata
            Scope: README.md, docs/PLAY.md, package metadata/display, RELEASE-NOTES.md; built app/version checks.
            Depends on: T-009, T-010 and the preceding milestone/phase prerequisite.
            Outcome: Document verified setup/play commands and baseline keys/limits; align package/display/notes at 0.1.0 and prepare complete delivered-scope notes. Do not tag or publish from the phase branch.
            Contracts: R16–R18.
            Evidence: Document examples/links checked against actual commands; package/display/notes match, baseline browser/build evidence linked and no later features claimed.
            Folded milestone exit (part of this implementation task): review the whole 1.3 capability, execute required checks/regressions, repair blockers, retain TODO dispositions and demonstrate its PLAN outcome before this task completes.
            Report: docs/dev/reports/phases/1/1.3.md; refresh docs/dev/HANDOFF.md in the same completion commit. Close/read back the milestone after all task issue closures.
    - [ ] Milestone 1.4 — Phase review
        - [ ] T-012 — Review test and report the classic baseline
            Scope: All Phase 1 source/tests/docs; docs/dev/reports/phases/1/PHASE-REPORT.md and HANDOFF.md.
            Depends on: T-005, T-008, T-011; complete and verify closure of every delivery milestone in Phase 1 before this task starts.
            Outcome: Review cross-milestone code, classic acceptance, browser/build/package evidence and TODOs after delivery closures. Repair blockers and prepare the phase integration/release handoff.
            Contracts: R01–R05, R16–R18 baseline.
            Evidence: Complete scoped phase exits, report/HANDOFF and one owning checkbox commit; verify task publication/issue closure and phase review milestone closure. Coordinator then explicitly integrates, checks/publishes merged default state and releases 0.1.0 with tag/body readback before Phase 2 activation.
            Report: docs/dev/reports/phases/1/PHASE-REPORT.md; refresh docs/dev/HANDOFF.md.

## Phase 2 — Standard assistance

- [ ] Phase 2 — Standard assistance
    - [ ] Milestone 2.1 — Predictable source and rotation
        - [ ] T-013 — Implement seven-bag generation and stable lookahead
            Scope: src/domain/pieces.ts and game.ts; tests/unit/domain/pieces.test.ts.
            Depends on: T-012.
            Outcome: Replace the baseline source with injected-random seven-bag shuffle and nonconsuming next lookup; new game resets source state. Preserve spawn/lock chronology.
            Contracts: R06.
            Evidence: Aligned bags contain all seven types, seeded sequences are stable, bag boundary lookahead and repeated preview calls do not consume pieces.
        - [ ] T-014 — Implement ordered SRS rotation candidates
            Scope: src/domain/geometry.ts and game.ts; geometry/board fixtures.
            Depends on: T-013.
            Outcome: Apply exact I/JLSTZ orientation/kick tables and first-legal candidate behavior, with unchanged O footprint and rejected-rotation timing.
            Contracts: R01, R04, R10.
            Evidence: All eight ordered transitions per family, wall/floor/hidden/obstruction/all-fail fixtures, O no-op and no grounded deadline reset.
        - [ ] T-015 — Integrate stable next preview and rotation feedback
            Scope: src/ui/panel.ts, controller snapshots, production integration and tests/e2e; player help.
            Depends on: T-013, T-014 and the preceding milestone/phase prerequisite.
            Outcome: Render the next piece without source mutation and demonstrate actual-browser bag/kick behavior while preserving classic controls. Update help for the delivered preview/rotation behavior.
            Contracts: R06, R10, R16–R17.
            Evidence: Visible next/type consistency, source order unaffected by redraw, real wall/floor kick routes in both browsers and baseline regression checks.
            Folded milestone exit (part of this implementation task): review the whole 2.1 capability, execute required checks/regressions, repair blockers, retain TODO dispositions and demonstrate its PLAN outcome before this task completes.
            Report: docs/dev/reports/phases/2/2.1.md; refresh docs/dev/HANDOFF.md in the same completion commit. Close/read back the milestone after all task issue closures.
    - [ ] Milestone 2.2 — Hold workflow
        - [ ] T-016 — Implement Hold exchange and per-lock eligibility
            Scope: src/domain/pieces.ts and game.ts; unit state traces.
            Depends on: T-015.
            Outcome: Implement empty/populated Hold, source consumption rules, spawn orientation/position/timer reset and once-per-lock eligibility. Rejected Hold leaves state unchanged; handle held replacement spawn collision.
            Contracts: R01, R04, R06–R07.
            Evidence: Empty store/next consumption, populated exchange/no source consumption, repeated rejection, lock resets eligibility and collision top-out.
        - [ ] T-017 — Expose Hold panel and keyboard command
            Scope: src/ui/panel.ts, src/browser/keyboard.ts, controller/snapshots; browser checks.
            Depends on: T-016.
            Outcome: Connect C to Hold and show held type/availability using production session snapshots. Clear held input repeats across replacement, preserve native form/lifecycle handling.
            Contracts: R02–R03, R07, R16.
            Evidence: Visible held type and availability after store/exchange/lock, edge-triggered C, form focus and pause rejection.
        - [ ] T-018 — Integrate Hold with session timing and player guidance
            Scope: tests/unit/app/controller.test.ts, tests/e2e Hold routes, docs/PLAY.md.
            Depends on: T-016, T-017 and the preceding milestone/phase prerequisite.
            Outcome: Implement integrated Hold/source/timer regression scenarios and accurate user guidance. Verify replacement gets fresh timing but cannot grant an extra Hold before lock.
            Contracts: R02–R04, R06–R07, R16–R17.
            Evidence: Browser empty/populated/collision routes and deterministic source/timer traces; prior bag/kick/classic behavior preserved.
            Folded milestone exit (part of this implementation task): review the whole 2.2 capability, execute required checks/regressions, repair blockers, retain TODO dispositions and demonstrate its PLAN outcome before this task completes.
            Report: docs/dev/reports/phases/2/2.2.md; refresh docs/dev/HANDOFF.md in the same completion commit. Close/read back the milestone after all task issue closures.
    - [ ] Milestone 2.3 — Landing assistance
        - [ ] T-019 — Implement shared normal landing and ghost rendering
            Scope: src/domain/landing.ts, game/controller snapshots, src/ui/board.ts; landing/render tests.
            Depends on: T-018.
            Outcome: Introduce a single normal landing query and always-visible legal ghost. Suppress non-visible/no-result landing and render behind a coincident active piece.
            Contracts: R01, R08.
            Evidence: Floor/stack/top-out/no-result fixtures, no board/source mutation, distinguishable outline and exact pose projection.
        - [ ] T-020 — Implement hard drop with preserved lock deadline
            Scope: src/domain/game.ts, src/browser/keyboard.ts; focused tests.
            Depends on: T-019.
            Outcome: Connect Space to shared landing, two points per descended row and full-interval follow locking. Preserve an existing grounded deadline and reject zero-effect delay resets.
            Contracts: R02, R04–R05, R08–R09.
            Evidence: Drop/ghost equality, distance points, newly grounded interval, existing deadline, repeated zero-distance drop and input-at-deadline traces.
        - [ ] T-021 — Integrate assistance routes and release documentation
            Scope: tests/e2e, controller regression checks, docs/PLAY.md, package/display version, RELEASE-NOTES.md.
            Depends on: T-019, T-020 and the preceding milestone/phase prerequisite.
            Outcome: Implement complete bag/Hold/SRS/ghost/drop routes and cross-feature acceptance; align delivered help/notes/version at 0.2.0.
            Contracts: R06–R10, R16–R18.
            Evidence: Both browser input/render routes, source/Hold/lock regressions, clean type/unit/build and honest always-show/full-interval release scope.
            Folded milestone exit (part of this implementation task): review the whole 2.3 capability, execute required checks/regressions, repair blockers, retain TODO dispositions and demonstrate its PLAN outcome before this task completes.
            Report: docs/dev/reports/phases/2/2.3.md; refresh docs/dev/HANDOFF.md in the same completion commit. Close/read back the milestone after all task issue closures.
    - [ ] Milestone 2.4 — Phase review
        - [ ] T-022 — Review test and report standard assistance
            Scope: Phase 2 source/tests/docs; docs/dev/reports/phases/2/PHASE-REPORT.md and HANDOFF.md.
            Depends on: T-015, T-018, T-021; complete and verify closure of every delivery milestone in Phase 2 before this task starts.
            Outcome: Review source/Hold/rotation/landing interactions and prior classic regressions, resolve blockers and prepare verified 0.2.0 integration/release evidence.
            Contracts: R01–R10, R16–R18 delivered scope.
            Evidence: Delivery milestone closures first; phase report/TODOs and publication/closure verified. Coordinator integrates, verifies merged default and publishes/readbacks 0.2.0 before Phase 3.
            Report: docs/dev/reports/phases/2/PHASE-REPORT.md; refresh docs/dev/HANDOFF.md.

## Phase 3 — Penetrator play

- [ ] Phase 3 — Penetrator play
    - [ ] Milestone 3.1 — Safe penetrator traversal
        - [ ] T-023 — Implement deepest legal penetrator landing queries
            Scope: src/domain/landing.ts and board/geometry queries; focused fixtures.
            Depends on: T-022.
            Outcome: Enumerate deepest fully visible nonoverlapping landing at/below pose despite intervening blocks. Expose no-result and legal-transit bounds without changing normal landing.
            Contracts: R01, R08–R11.
            Evidence: Barrier/lower cavity, occupied floor/higher pocket, overlapping current transit, all-blocked/no-result, side/floor/hidden bounds and exact normal-versus-penetrator endpoints.
        - [ ] T-024 — Implement penetrator activation transit and contact timing
            Scope: src/domain/game.ts; synthetic bonus/time/movement traces.
            Depends on: T-023.
            Outcome: Validate/spend a fixture bonus exactly once, allow one-row transit through occupied cells, use deepest landing grounding and cancel obsolete normal contact timer. Reject moves/rotations losing all legal landings and reject Hold.
            Contracts: R04, R07, R09–R11.
            Evidence: Activation prerequisites/cost/repeat rejection, transit without board erasure, final deadline, invalid action unchanged state and safe lock validation. Fixture bonus injection remains test-only.
        - [ ] T-025 — Integrate penetrator input white rendering and projection
            Scope: src/browser/keyboard.ts, controller snapshots, src/ui/board.ts and panel status; browser integration.
            Depends on: T-023, T-024 and the preceding milestone/phase prerequisite.
            Outcome: Connect B, white active/locked cells and current penetrator status. Use shared penetrator landing for ghost/hard drop and show rejected Hold behavior.
            Contracts: R02, R07–R12, R16–R17.
            Evidence: Actual composed barrier/cavity scenario, ghost/drop equality, white-versus-regular color observations and valid final placement; review complete 3.1 capability without claiming naturally earned bonus UI.
            Folded milestone exit (part of this implementation task): review the whole 3.1 capability, execute required checks/regressions, repair blockers, retain TODO dispositions and demonstrate its PLAN outcome before this task completes.
            Report: docs/dev/reports/phases/3/3.1.md; refresh docs/dev/HANDOFF.md in the same completion commit. Close/read back the milestone after all task issue closures.
    - [ ] Milestone 3.2 — Earned bonuses and conditional projection
        - [ ] T-026 — Award capped bonuses through production line clearing
            Scope: src/domain/game.ts; line-clear/bonus tests.
            Depends on: T-025.
            Outcome: Award one bonus on exactly four cleared rows, cap at three and preserve cost-before-award ordering. Permit penetrator clears to earn and reset bonuses on restart.
            Contracts: R05, R11–R12.
            Evidence: 0/2/3 count transitions, fewer lines, spend then earn, penetrator clear and rejected activation without spend.
        - [ ] T-027 — Display earned and spent bonus symbols accessibly
            Scope: src/ui/panel.ts and controller snapshot integration; browser checks.
            Depends on: T-026.
            Outcome: Render one lightning symbol per available bonus, readable zero and accessible numeric label. Update atomically on earning/spending/restart.
            Contracts: R12, R16.
            Evidence: Exact 0–3 symbol/label counts, earn→activate→lock sequence using production rules, no stale count after restart.
        - [ ] T-028 — Integrate dynamic ghost threshold and penetrator release routes
            Scope: src/ui/board.ts, controller projection visibility, tests/e2e and fixtures, docs/PLAY.md, package/display/notes.
            Depends on: T-026, T-027 and the preceding milestone/phase prerequisite.
            Outcome: Apply fixed dynamic visibility default: hide unless every projected cell is in rows 10–19. Integrate earned-bonus traversal/drop/lock with threshold feedback; align help/metadata/notes at 0.3.0.
            Contracts: R08–R12, R16–R18.
            Evidence: Above/below/straddling/exact-row-ten and coincident ghost cases, ordinary/penetrator equality, hold/timer/bonus regressions and both-browser white/indicator observations.
            Folded milestone exit (part of this implementation task): review the whole 3.2 capability, execute required checks/regressions, repair blockers, retain TODO dispositions and demonstrate its PLAN outcome before this task completes.
            Report: docs/dev/reports/phases/3/3.2.md; refresh docs/dev/HANDOFF.md in the same completion commit. Close/read back the milestone after all task issue closures.
    - [ ] Milestone 3.3 — Phase review
        - [ ] T-029 — Review test and report penetrator play
            Scope: Phase 3 source/tests/docs; docs/dev/reports/phases/3/PHASE-REPORT.md and HANDOFF.md.
            Depends on: T-025, T-028; complete and verify closure of every delivery milestone in Phase 3 before this task starts.
            Outcome: Review legal-transit/lock separation, bonus lifecycle, ghost/drop visibility and preserved assistance. Repair blockers and prepare 0.3.0 integration/release evidence.
            Contracts: R01–R12, R16–R18 delivered scope.
            Evidence: Delivery closures, code review and required browser/domain regressions, report/TODOs/HANDOFF and publication/closure. Coordinator integrates, merged-state verifies and publishes/readbacks 0.3.0 before Phase 4.
            Report: docs/dev/reports/phases/3/PHASE-REPORT.md; refresh docs/dev/HANDOFF.md.

## Phase 4 — Session configuration

- [ ] Phase 4 — Session configuration
    - [ ] Milestone 4.1 — Immediate display and action controls
        - [ ] T-030 — Introduce typed selected session settings
            Scope: src/app/config.ts, controller/snapshot contracts; unit tests.
            Depends on: T-029.
            Outcome: Define the six keys/allowed typed values/defaults and selected state, initialized session-only. Preserve current full-interval/follow policies until 4.2 exposes alternatives; do not add browser persistence.
            Contracts: R13.
            Evidence: All defaults/types, no board/source/Hold/totals changes on configuration updates, selected values survive restart but reset on reload.
        - [ ] T-031 — Expose four immediate display and action controls
            Scope: src/ui/panel.ts, config/controller integration, src/ui/board.ts and keyboard eligibility.
            Depends on: T-030.
            Outcome: Add labeled ghost/hard-drop/next-preview/Hold controls. Apply immediate effects while preserving source/held type/used flag; support hide/dynamic/show and disabled-action rejection.
            Contracts: R07–R09, R13, R16.
            Evidence: Every mode/toggle while ready/running/paused, hidden preview no source consumption, disabled hard-drop/Hold unchanged state and native form keyboard use.
        - [ ] T-032 — Integrate live control behavior and session guidance
            Scope: tests/unit/app/controller.test.ts, tests/e2e controls routes, docs/PLAY.md.
            Depends on: T-030, T-031 and the preceding milestone/phase prerequisite.
            Outcome: Implement cross-control lifecycle/penetrator regression scenarios and session-only help. Verify controls do not advance paused time or grant an extra Hold.
            Contracts: R02–R03, R07–R09, R11, R13, R16–R17.
            Evidence: Both-browser controls/focus/visibility routes, retained held state and complete 4.1 integration; help accurately limits persistence and pending policy capability.
            Folded milestone exit (part of this implementation task): review the whole 4.1 capability, execute required checks/regressions, repair blockers, retain TODO dispositions and demonstrate its PLAN outcome before this task completes.
            Report: docs/dev/reports/phases/4/4.1.md; refresh docs/dev/HANDOFF.md in the same completion commit. Close/read back the milestone after all task issue closures.
    - [ ] Milestone 4.2 — Configurable lock policies
        - [ ] T-033 — Implement selected and captured lock-policy semantics
            Scope: src/domain/game.ts, src/app/config.ts and controller.ts; policy/time tests.
            Depends on: T-032.
            Outcome: Add failed-down general mode and immediate-hard-drop override. Capture selected policies at every source/Hold spawn; current piece/deadline remains unchanged on selecting a different policy.
            Contracts: R04, R07, R09, R11, R13.
            Evidence: General×drop policy combinations for normal/penetrator, same-time commands, hard-drop existing deadline, Hold replacement capture and activation grounding evaluation.
        - [ ] T-034 — Expose lock controls with next-piece feedback
            Scope: src/ui/panel.ts, controller snapshots; focused browser checks.
            Depends on: T-033.
            Outcome: Add general/hard-drop policy controls and visible selected/current-piece/pending indication. Pending clears when an appropriate new piece captures the selected values.
            Contracts: R04, R09, R13, R16.
            Evidence: Change while grounded/paused/ready, no retroactive lock/reset, normal/Hold spawn capture and labels that agree with the actual piece policies.
        - [ ] T-035 — Verify six-control interaction matrix and release guidance
            Scope: tests/unit/app and tests/e2e, docs/PLAY.md, package/display version, RELEASE-NOTES.md.
            Depends on: T-033, T-034 and the preceding milestone/phase prerequisite.
            Outcome: Implement integrated all-setting/policy normal/penetrator lifecycle scenarios. Document session-only behavior and align 0.4.0 version/help/notes.
            Contracts: R01–R13, R16–R18 delivered scope.
            Evidence: Both-browser input/controls/rendering, deterministic policy boundaries, no erasure of held/source/board state, clean builds and no persistence claim.
            Folded milestone exit (part of this implementation task): review the whole 4.2 capability, execute required checks/regressions, repair blockers, retain TODO dispositions and demonstrate its PLAN outcome before this task completes.
            Report: docs/dev/reports/phases/4/4.2.md; refresh docs/dev/HANDOFF.md in the same completion commit. Close/read back the milestone after all task issue closures.
    - [ ] Milestone 4.3 — Phase review
        - [ ] T-036 — Review test and report session configuration
            Scope: Phase 4 source/tests/docs; docs/dev/reports/phases/4/PHASE-REPORT.md and HANDOFF.md.
            Depends on: T-032, T-035; complete and verify closure of every delivery milestone in Phase 4 before this task starts.
            Outcome: Review all six settings and timing/penetrator/Hold interactions against preserved game behavior; repair blockers and prepare 0.4.0 integration/release evidence.
            Contracts: R01–R13, R16–R18 delivered scope.
            Evidence: Delivery closures, applicable matrix/browser/build checks, report/TODOs/HANDOFF and publication/closure. Coordinator integrates, verifies merged state and publishes/readbacks 0.4.0 before Phase 5.
            Report: docs/dev/reports/phases/4/PHASE-REPORT.md; refresh docs/dev/HANDOFF.md.

## Phase 5 — Persistent preferences

- [ ] Phase 5 — Persistent preferences
    - [ ] Milestone 5.1 — Correct restoration
        - [ ] T-037 — Implement exact setting codecs and raw-source distinctions
            Scope: src/app/config.ts, tests/unit/app/config.test.ts.
            Depends on: T-036.
            Outcome: Add canonical six-key serialization and absent/valid/invalid/duplicate source parsing, exact case/value validation and safe decode behavior. Unknown data remains unrelated.
            Contracts: R13–R15.
            Evidence: Every key with exact/default/empty/whitespace/case/unknown/malformed/duplicate inputs, canonical encoding and no accepted out-of-range strings.
        - [ ] T-038 — Resolve settings independently before initial spawn
            Scope: src/browser/preferences.ts readers, src/app/config.ts and controller startup; focused tests.
            Depends on: T-037.
            Outcome: Read initial query/cookie snapshots and resolve each key URL→cookie→default with invalid fallthrough and duplicate-cookie handling. Apply before session construction without simulated user-change writes.
            Contracts: R14–R15.
            Evidence: Per-key matrix and mixed-source fixture, omitted key retaining cookie, unreadable cookie fallback, initial visible controls/captured policies and unknown input preservation.
        - [ ] T-039 — Persist valid explicit URL selections during restore
            Scope: src/app/controller.ts, src/browser/preferences.ts cookie writer, tests/e2e restore routes.
            Depends on: T-037, T-038 and the preceding milestone/phase prerequisite.
            Outcome: Write cookies for each valid explicit URL setting including defaults, using exact attributes. Do not overwrite cookies for invalid or absent URL inputs. Integrate restored controls and initial timing.
            Contracts: R13–R15, R16–R17.
            Evidence: Explicit-default conflict→reload, omitted URL→cookie retained, malformed/duplicate URL no overwrite, six-key restored view and valid URL still usable with denied cookies.
            Folded milestone exit (part of this implementation task): review the whole 5.1 capability, execute required checks/regressions, repair blockers, retain TODO dispositions and demonstrate its PLAN outcome before this task completes.
            Report: docs/dev/reports/phases/5/5.1.md; refresh docs/dev/HANDOFF.md in the same completion commit. Close/read back the milestone after all task issue closures.
    - [ ] Milestone 5.2 — Durable changes and resilient sharing
        - [ ] T-040 — Persist user changes without reloading or losing URL data
            Scope: src/app/controller.ts and config.ts, src/browser/preferences.ts; focused tests.
            Depends on: T-039.
            Outcome: Connect actual selected-value changes to independent cookie and replaceState writes. Canonicalize only the changed recognized parameter, preserve other keys/unknown data/fragment and avoid navigation/history growth.
            Contracts: R13–R15.
            Evidence: One key at a time, no-op selection, duplicate changed key normalization, no reload, cookie attributes and query/fragment preservation; session state remains immediate.
        - [ ] T-041 — Report persistence failures while preserving play
            Scope: src/browser/preferences.ts, controller result model, src/ui/panel.ts; unit/browser failures.
            Depends on: T-040.
            Outcome: Detect thrown/denied/silently discarded cookie writes and failed history operations. Attempt destinations independently and show concise nonblocking truthful feedback without exposing raw cookie/URL data.
            Contracts: R15–R16.
            Evidence: Getter/setter/history exceptions, silent rejection, both failures, one successful destination, continued play/settings and truthful persistence/sharing status.
        - [ ] T-042 — Verify reload sharing and full-target static distribution
            Scope: tests/e2e, adapter fixtures, built output checks, README.md/docs/PLAY.md, package/display metadata and RELEASE-NOTES.md.
            Depends on: T-040, T-041 and the preceding milestone/phase prerequisite.
            Outcome: Implement actual-browser reload/share/invalid/failure routes for all six settings and full prior-game regressions. Validate final static package/subpath and documented commands; align delivered full-target version and notes at 1.0.0.
            Contracts: R01–R18.
            Evidence: Chromium/Firefox application routes and two-cycle teardown, per-key/mixed precedence/default writes, independent-failure evidence, clean type/unit/build/e2e results, version consistency and credential/browser/test-output exclusion.
            Folded milestone exit (part of this implementation task): review the whole 5.2 capability, execute required checks/regressions, repair blockers, retain TODO dispositions and demonstrate its PLAN outcome before this task completes.
            Report: docs/dev/reports/phases/5/5.2.md; refresh docs/dev/HANDOFF.md in the same completion commit. Close/read back the milestone after all task issue closures.
    - [ ] Milestone 5.3 — Phase review
        - [ ] T-043 — Review test and report the complete target
            Scope: Full source/tests/docs; docs/dev/reports/phases/5/PHASE-REPORT.md, docs/dev/reports/IMPLEMENTATION-REPORT.md and HANDOFF.md.
            Depends on: T-039, T-042; complete and verify closure of every delivery milestone in Phase 5 before this task starts.
            Outcome: Review full-target contracts/integration and package/release readiness. Repair blockers; aggregate every unresolved/deferred milestone/phase TODO with provenance/options/owner into final report. Prepare the 1.0.0 integration/release handoff.
            Contracts: R01–R18.
            Evidence: All delivery milestones complete/closed; full contract and browser/build evidence, final report/TODO aggregation, phase report/HANDOFF and task publication/closure. Coordinator explicitly integrates, checks/publishes merged default, then tags/publishes/readbacks 1.0.0; report remaining operations honestly.
            Report: docs/dev/reports/phases/5/PHASE-REPORT.md; refresh docs/dev/HANDOFF.md.
            Final report: docs/dev/reports/IMPLEMENTATION-REPORT.md, including all deferred TODO provenance or TODO=None.

