# Task-list review report

## Current gate

State: **Ready as a task breakdown**, owner sdd-tasks, reviewed 2026-10-24. Scope: TASKS against current Ready SPEC/design and combined PLAN/layout. This does not authorize implementation, phase activation, package-wide QC or integration.

## Exact reviewed and governing states

SHA-256 identities; TASKS is reviewed and the other files are governing inputs/evidence.

| Source | SHA-256 |
| --- | --- |
| TASKS.md | `af7b31e808d6ac10c56ac9a4acdb14b841978017c0db0222d2d4e4d56325f5e4` |
| PLAN.md | `91dd7547f597f7ab22f090c96e7bf53055f75efa7b1bcf5ec3bf3f98e20c9be8` |
| layout.md | `5296f20df57ed17b711cd7d1003b834588fd7648579899c47ce7767cbd6077e3` |
| PLAN-REVIEW-REPORT.md | `8a2e565a87eafdf318c2f0d7f8cd22fbbe84185d6284ee5a06b08b2d3ca7c069` |
| SPEC.md | `e72151dbc4c069f29cd0b93cc0b30b6ea8e2c7e83f91f3878522eb2be3c769fd` |
| spec/GEOMETRY.md | `fb1aca816a0d1e312634ff020cb0baff67ed33012b28518204d11f83b96d2dca` |
| spec/CONFIGURATION.md | `d629a733cd31b4449ae1375d2e4f1663b53d2fdec4d7b0b9b6ecd6383c59e978` |
| SPEC-REVIEW-REPORT.md | `5d9e016424c53ec268ba21ba6f3853b038e37ab9ace39aff7c8363db75049414` |
| PROJECT.md | `4546581d3e37fc853b5d858ee22492de8f17d1a36959dbb5d613b256fdc2f1e7` |
| ARCHITECTURE.md | `97f1b1df98b1e2015bd0b78924ec3199483502f405d7ca37eb01ec500fe7eabf` |
| DECOMPOSITION.md | `6c8ed74b1e0bd1cbddf1d71c5dc6479d2f3cc29daca9127f97b5f7c8f0c16543` |

## Initial conformance review

| Delivery milestone | Delivery tasks | Excluded review | Final boundary | Semantic assessment |
| --- | --- | --- | --- | --- |
| 1.1 Falling-piece play path | 4 (T-001–T-004) | T-005 | T-005 standalone | Setup/readiness, geometry/board, temporal normal session and production browser composition have distinct verifiable outcomes. T-004 touches several adapters but only integrates the bounded falling/locking path; it does not own complete classic scoring/lifecycle or modern enhancements. |
| 1.2 Complete classic game loop | 3 (T-006–T-008) | None | T-008 folded | Clearing/scoring, lifecycle/suspension and visible/native controls complete the earliest classic use path. |
| 1.3 Reproducible baseline distribution | 3 (T-009–T-011) | None | T-011 folded | Actual-browser acceptance, static CI/build and verified user/release instructions supply necessary quality/distribution rather than new feature scope. |
| 2.1 Predictable source and rotation | 3 (T-013–T-015) | None | T-015 folded | Source and kicks can be checked independently, then preview integrates them visibly. |
| 2.2 Hold workflow | 3 (T-016–T-018) | None | T-018 folded | State/source semantics, visible command/panel and integration/help remain bounded to Hold. |
| 2.3 Landing assistance | 3 (T-019–T-021) | None | T-021 folded | Shared projection, timed hard drop and cross-feature browser/release acceptance avoid separate landing algorithms. |
| 3.1 Safe penetrator traversal | 3 (T-023–T-025) | None | T-025 folded | Query legality, activation/transit timing and white input/render integration separate critical safety seams; test-only bonus setup is explicitly limited. |
| 3.2 Earned bonuses and conditional projection | 3 (T-026–T-028) | None | T-028 folded | Award/cap, visible indicator and threshold/complete-route acceptance form natural localized changes; T-028's visibility change is small with focused integration obligations. |
| 4.1 Immediate display and action controls | 3 (T-030–T-032) | None | T-032 folded | Typed session model, four live controls and behavior/help integration; no persistence invented. |
| 4.2 Configurable lock policies | 3 (T-033–T-035) | None | T-035 folded | Captured rules, truthful controls and normal/penetrator policy regression matrix are coherent, inspectable boundaries. |
| 5.1 Correct restoration | 3 (T-037–T-039) | None | T-039 folded | Codec/raw distinctions, per-key read/resolve and explicit URL cookie writes implement restore semantics without bundling change-time effects. |
| 5.2 Durable changes and resilient sharing | 3 (T-040–T-042) | None | T-042 folded | Change effects, failure feedback and real-browser/full-target distribution checks finish preferences; T-042 tests prior behavior rather than implementing an unrelated remaining subsystem. |

All delivery task counts are in the preferred 3–5 range, but semantic breadth was assessed separately as above. No task group has 1–2 or ≥10 delivery tasks. Phase counts and the justified two-milestone phases retain PLAN review's reasoning; tasks do not invent extra milestones to meet a quota. Total: 37 delivery tasks, one standalone milestone review and five phase review tasks = 43 tasks. No extra administrative milestone reviews were inserted below the threshold.

## Structure, dependencies and review obligations

Executed the installed forge-project-draft canonical parser locally for each of Phases 1–5. It accepted the complete hierarchy and selected counts 12/10/7/7/7 tasks with 4/4/3/3/3 milestones. Draft eligibility fields remained false; no hosted phase object was created or activated. Separate checks confirmed monotonic unique T-001–T-043 IDs, one owning parent, exact four/eight-space checkbox nesting, phase heading/checkbox agreement, exact ordered PLAN milestone IDs/names and unchecked implementation state. Dependency references target earlier known IDs; all five phase review tasks name every delivery boundary and require all delivery milestone closures.

Verified threshold: 1.1 has four delivery tasks and multiple delivery milestones in Phase 1, so T-005 is a standalone review. Every other delivery milestone has three tasks, so its final delivery task owns code review/tests/repairs/report/HANDOFF. Eleven folded boundaries and seventeen milestone report owners are present. Phase review milestones 1.4/2.4/3.3/4.3/5.3 each contain exactly one review task. Reports use the lifecycle phase prefix. T-043 additionally owns the final implementation report and TODO provenance aggregation.

## Coverage and limits

Task contract fields route R01–R18 through PLAN's unchanged phase scope. Cross-component behavior is integrated within milestones rather than left to the final phase: source/Hold/landing, transit/valid locking, selected/captured policies and restore/change-time persistence. Each task has expected physical owners, prerequisites, observable outcome and focused evidence. Failure paths, setup/browser diagnostics, actual application rendering, source/test fixture isolation, player documentation, static subpath/package/version evidence and coordinator-owned releases all have explicit work. Generated paths remain planned, not implemented files.

No requirements were added by TASKS, no phase strategy was changed, and no completed work was inferred. Manual review checked bounds/cohesion, contract and failure coverage, earliest usable path, lifecycle thresholds and integration/release sequencing. Mechanical parser/DAG/count/link/style checks support, rather than replace, that review. No confirmed conformance findings remain; no correction/recheck Revision section is required. Ready applies only to the task breakdown. No product code, application tests, build, browser installation or release was executed during this review.
