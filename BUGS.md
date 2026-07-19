# Bug Log — OriginScrub

Every bug found while running, compiling, building, testing, or reviewing OriginScrub is logged here — no exceptions, no matter how small or how quickly fixed.

## Rules (do not violate)

1. **Append-only:** never delete a bug record once added. Closed bugs stay in the log permanently. Only the `Status`, `Resolution`, and `Closed` fields of an existing record may be updated.
2. IDs are sequential (`BUG-001`, `BUG-002`, …) and are never reused or renumbered.
3. Every bug references the `TASKS.md` task (or phase) it was found during, and the fix (when made) is recorded in `CHANGELOG.md` referencing the bug ID.
4. A bug is not `Fixed` until the fix is verified (test passes / behavior confirmed) — until then it stays `In Progress`.

## Status values

`Open` → `In Progress` → `Fixed` | `Won't Fix` | `Duplicate of BUG-xxx` | `Not a Bug`

## Severity values

- **Critical** — data loss beyond target origin, security hole, false success reporting, release blocker.
- **High** — core flow broken, no workaround.
- **Medium** — feature defect with workaround, incorrect messaging.
- **Low** — cosmetic, minor UX, dev-only annoyance.

## Index

| ID | Date | Severity | Task Ref | Title | Status |
|---|---|---|---|---|---|
| — | — | — | — | _No bugs logged yet_ | — |

---

## Records

_No bugs logged yet. Use the template below for the first record._

<!-- TEMPLATE — copy below this line for each new bug, append at the END of this section, and add a row to the Index table above.

### BUG-NNN — Short title

- **Date found:** YYYY-MM-DD
- **Found during:** task ID from TASKS.md (e.g. 2.1.1) / activity (build, unit test, e2e, manual test, review)
- **Severity:** Critical | High | Medium | Low
- **Environment:** OS, browser + version, build mode (dev/prod), relevant tool versions
- **Status:** Open
- **Description:** What is wrong.
- **Steps to reproduce:**
  1. …
- **Expected:** …
- **Actual:** … (include exact error output/log where available)
- **Resolution:** — (fill when closed: what was changed, root cause)
- **Fixed in:** — (commit hash / CHANGELOG entry date)
- **Closed:** — (YYYY-MM-DD)

-->
