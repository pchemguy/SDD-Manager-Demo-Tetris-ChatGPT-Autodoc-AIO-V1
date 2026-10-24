# Repository layout

This map allocates the logical owners from [DECOMPOSITION](DECOMPOSITION.md) to physical homes and supports [PLAN](PLAN.md). Product source, test and build paths below are intended locations, not claims that implementation files exist. The repository currently contains governing guidance and development documents.

## Ownership map

| Path / area | Owner and allowed responsibility | Checks / consumers |
| --- | --- | --- |
| src/domain/types.ts | Domain piece/pose/grid/session/action types; no DOM or preference transport types | Other domain modules and read-only snapshot consumers |
| src/domain/geometry.ts | C-GEOMETRY masks, cell transforms and kick candidates | tests/unit/domain/geometry.test.ts |
| src/domain/board.ts | C-BOARD pure grid/placement/clear operations | tests/unit/domain/board.test.ts |
| src/domain/landing.ts | C-LANDING normal/penetrator projections and grounding queries | tests/unit/domain/landing.test.ts |
| src/domain/pieces.ts | C-PIECES injected randomizer/source/queue and Hold bookkeeping helpers | tests/unit/domain/pieces.test.ts |
| src/domain/game.ts | C-GAME transitions, timers, lock/score/bonus and active-piece policy snapshot | tests/unit/domain/game.test.ts; controller integration checks |
| src/app/config.ts | C-CONFIG typed six-setting model, codecs/defaults, per-key resolution and selected/pending model | tests/unit/app/config.test.ts |
| src/app/controller.ts | C-CONTROLLER session/settings orchestration, ports and serialized time/intents | tests/unit/app/controller.test.ts |
| src/browser/keyboard.ts | C-INPUT event/filter/repeat adapter | tests/unit/browser/keyboard.test.ts; real e2e input |
| src/browser/clock.ts | Monotonic animation/visibility lifecycle adapter | tests/unit/browser/clock.test.ts; lifecycle browser checks |
| src/browser/preferences.ts | C-PREFERENCES raw cookie/history operations and confirmed effect results | tests/unit/browser/preferences.test.ts; browser reload/cookie/query checks |
| src/ui/board.ts | C-RENDER Canvas view of immutable snapshots | tests/e2e rendering observations and focused renderer fixtures |
| src/ui/panel.ts | C-PANEL HTML status, score/Hold/preview/bonus and controls | tests/e2e controls/focus/feedback |
| src/ui/styles.css | Board/panel sizing, contrast, square cells and responsive desktop layout | 1024×768 and 1440×900 browser observations |
| src/main.ts, index.html | Composition and static page mount; construct adapters/controller, connect views, own teardown | Actual application served in e2e and built-subpath checks |
| tests/fixtures/ | Synthetic board/piece/time/random factories; no production state mutation interface | Deterministic domain/controller and test-only browser scenarios |
| tests/e2e/ | Playwright actual-application routes, visual observations and reload/failure scenarios | Isolated managed Chromium and Firefox projects |
| tests/harness/ | Optional test-only entry composing real production modules with injected deterministic fixtures | Edge-case browser integration evidence; excluded from production build and clearly distinguished from built-application acceptance |
| package.json, package-lock.json | Locked development toolchain, npm script declarations and package version | Clean install/typecheck/unit/build/e2e checks |
| tsconfig.json, vite.config.ts | TypeScript/Vite configuration and static subpath asset output | Build/type checks; no UI framework |
| vitest.config.ts, playwright.config.ts | Unit runner and dedicated managed-browser test projects/timeouts | Reproducible host/browser diagnostics and execution |
| .github/workflows/ci.yml | Locked dependency checks and isolated browser provisioning/execution | Actual CI run evidence when implementation enables it |
| README.md, docs/PLAY.md | Truthful status, verified install/build/play instructions, keys/settings/version limitations | Link/example checks and actual declared commands |
| RELEASE-NOTES.md | Finalized notes for the current eligible implementation release | Version/source/body consistency and GitHub readback |

Types shared only by browser/preferences ports belong at the app boundary, not in domain types. Internal helper extraction is allowed within an area's ownership when cohesion warrants it; it must preserve the dependency direction. A component's responsibilities need not be forced into a single file if implementation evidence warrants a focused helper.

## Dependency and test rules

Domain source imports only domain source and language facilities. Application source imports domain/contracts; it receives browser effects through ports. UI/input/adapters call controller contracts or consume immutable snapshots. Only main composes concrete adapters. Avoid generic shared/utils directories: put a helper with its owner until a real shared contract justifies extraction.

Unit checks use fake clocks/random sources/preference ports. Browser checks use real application input, rendering and persistence; fixtures for hard-to-reach geometry or four-line states remain in tests. A harness may reuse production modules with injected state, but cannot replace actual built-application lifecycle/control/reload acceptance. Do not ship a debug mutation API, test fixture route or secrets in the runtime bundle merely to simplify tests.

Planned toolchain: TypeScript, Vite, Vitest and Playwright. Resolve supported exact versions and record them with the lockfile during setup. Intended npm commands are dev, typecheck, test, build and test:e2e; they are not declared or verified commands until implementation creates and exercises their package scripts. Windows invocation must follow WINDOWS_SHELL_POLICY.md; Linux uses its native shell. No setup mutates unrelated toolchains.

## Documents and execution evidence

Current authoritative roots are PROJECT, ARCHITECTURE, DECOMPOSITION, SPEC with spec/GEOMETRY.md and spec/CONFIGURATION.md, PLAN, this layout and TASKS when created. Adjacent SPEC/PLAN/TASKS-REVIEW-REPORT.md files own their focused QC evidence. DOCS_TASKS and DOCS_PREPARATION own main preparation tasks and control/evidence respectively; HANDOFF links the current continuation boundary.

Implementation evidence locations are reserved as follows:

- docs/dev/reports/phases/1/ through docs/dev/reports/phases/5/: each delivery milestone uses its stable numeric milestone filename, for example 1/1.1.md; each phase uses PHASE-REPORT.md.
- docs/dev/reports/IMPLEMENTATION-REPORT.md: final full-target report with aggregated TODO provenance.
- docs/ai-logs/: manual historical records only, under its existing local AGENTS. No transcript work is implied by ordinary development.

Future child documents use stable uppercase descriptive filenames. Retain the existing canonical lowercase layout.md name. Do not create a new report registry or copy task checklists into reports.

## Generated and secret artifacts

Keep node_modules/, dist/, coverage/, test-results/, playwright-report/ and owned browser downloads/profiles outside tracked source. Add applicable ignore rules with implementation setup before generating them. Browser binary locations must be absolute dedicated paths selected for the project/job; a shared personal cache does not establish isolation. Test-only outputs and harness builds cannot enter the production package. Ignored gh.tkn remains a local credential input, never a dependency, bundle asset, report or release asset.

Source/test/documentation navigation is bidirectional through the ownership table and canonical roots. No unresolved placement blocks TASKS. Further paths should refine a real owner rather than introduce speculative infrastructure.
