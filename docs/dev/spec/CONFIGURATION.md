# Configuration contract

This child owns R13–R15. [SPEC](../SPEC.md) owns gameplay actions and their captured-policy semantics. Configuration stores preferences only; restart retains them while resetting game progress.

## R13 — Settings and live behavior

| Typed key | URL key | Cookie name | Allowed serialized values | Default | Application |
| --- | --- | --- | --- | --- | --- |
| ghost | ghost | tetris_ghost | hide, dynamic, show | dynamic | Current rendering |
| hardDrop | hardDrop | tetris_hardDrop | on, off | on | Current action eligibility |
| nextPreview | nextPreview | tetris_nextPreview | on, off | on | Current rendering, never source consumption |
| hardDropLock | hardDropLock | tetris_hardDropLock | follow, immediate | follow | Next spawned piece |
| lock | lock | tetris_lock | failed, interval | interval | Next spawned piece |
| hold | hold | tetris_hold | on, off | on | Current eligibility; stored type and used flag retained |

Each control has a persistent visible label and selected value. Lock controls additionally report current-piece policy and “applies to next piece” when selected and captured values differ. Initial spawn, source spawn and Hold replacement all capture selected lock values. No settings change rewrites the active piece's deadline, board, source, totals or bonus state. The hard-drop/hold enable switches remain immediate even when policies are pending. A change while ready/paused/game-over updates selected configuration without advancing game time.

Acceptance: change all six settings in ready/running/paused states; verify immediate effects, retained Hold and next-spawn policy capture, including a Hold replacement and a normal lock/spawn.

## R14 — Restore and persist

Parse the page's query and cookie snapshot once before initial session construction. Match keys and values case-sensitively after URL/cookie decoding. Accept only the listed exact strings; whitespace, empty values, unknown values or decoding errors are invalid. Unknown query/cookie names are unrelated data. Multiple occurrences of a recognized query key are invalid for that setting, even if equal. Multiple cookies with the same recognized name are also invalid for that setting, avoiding ambiguous path/domain duplicates.

For each key independently choose: one valid explicitly supplied URL value; otherwise one valid saved cookie; otherwise default. An invalid URL does not mask a valid cookie. A missing URL does not write a default over a cookie. For each valid explicit URL value, attempt to write its corresponding cookie, even if equal to the default or current cookie. Invalid URL values never trigger a cookie overwrite. Restoring from cookies/defaults need not add URL parameters. Initial restoration and form initialization must not masquerade as user-change events.

On an actual user change, update the selected value immediately, then independently attempt its cookie write and replace all occurrences of that URL key with one canonical serialized value. Preserve other settings' parameters, unknown parameters and the fragment. Use history.replaceState; do not reload or add a history entry. No-op selections need no write; valid explicit URL restoration still writes as above. A fresh page load re-resolves all keys. Live popstate/external URL edits during a session are not a configuration source; the displayed selected state remains until user changes or reload.

Write each cookie with Path=/, Max-Age=31536000 and SameSite=Lax; add Secure on HTTPS, omit on HTTP. No Domain attribute is required. Serialize names/values safely. Cookies are not game saves. Same-origin tabs/pages can read shared cookies; there is no automatic cross-tab synchronization during a session.

Acceptance matrix includes each of the six keys with absent URL/cookie, valid URL plus conflicting cookie, omitted URL plus cookie, explicit default plus conflicting cookie, invalid URL plus valid cookie, malformed/invalid cookie and duplicate values. A mixed-key fixture must resolve from different sources simultaneously. Reload after both control changes and explicit URL restoration must recover the chosen values. Preserve unrelated query data and fragment while canonicalizing only the changed key.

## R15 — Browser effect failure

Cookie reads/writes and history changes may fail, including silent cookie rejection. Catch adapter exceptions and keep the selected session value. Treat an unreadable cookie source as absent for resolution; valid URL inputs remain usable. Independently attempt the other write destination when one fails. Cookie write verification may read back the named value; if it cannot be confirmed, present concise nonblocking “preferences may not persist” feedback. A failed history update leaves the current address unchanged and reports that sharing the updated setting is unavailable. Never claim persistence succeeded solely because an assignment did not throw. Errors must not expose cookie contents or unrelated URL values in the UI/log.

Acceptance: simulate denied cookie getter/setter, silently discarded cookie writes, failed history.replaceState and both failures. Verify selected state and play remain usable, independent successful destination survives, feedback is truthful and no full-page navigation occurs.
