# OriginScrub — Task Tracker

**Source:** `OriginScrub_Implementation_Plan_v3.md` (Implementation Plan, Revision 3.0 — Hardened)
**Created:** 2026-07-20

## Rules (do not violate)

1. This file is **append-only for records**: never delete or remove a task row once added. Only the `Status`, `Updated`, and `Notes` cells of a row may change.
2. When a task is completed, set `Status` to ✅ Done, fill `Updated` with the date, and add a matching entry to `CHANGELOG.md`.
3. Any bug discovered while working a task is logged in `BUGS.md` and cross-referenced by task ID in the `Notes` cell.
4. New tasks discovered mid-implementation are **appended** to the correct module with the next free ID — existing IDs are never renumbered.
5. A phase's Exit Gate task may only be marked Done when every other task in that phase is Done (or formally deferred with a note).

## Status Legend

| Symbol | Meaning |
|---|---|
| ⬜ | Pending (not started) |
| 🟡 | In Progress |
| ✅ | Done |
| 🔴 | Blocked (explain in Notes) |
| ⏸ | Deferred (explain in Notes) |

## Dashboard

| Phase | Tasks | Done | Status |
|---|---:|---:|---|
| Phase 0 — Specification Lock & Scaffolding | 17 | 0 | ⬜ Not started |
| Phase 1 — Core Target & Validation Layer | 25 | 0 | ⬜ Not started |
| Phase 2 — Scrub Engine | 17 | 0 | ⬜ Not started |
| Phase 3 — Optional History Module | 14 | 0 | ⬜ Not started |
| Phase 4 — Popup & Interstitial UX | 20 | 0 | ⬜ Not started |
| Phase 5 — Security & Recovery | 17 | 0 | ⬜ Not started |
| Phase 6 — Store Compliance & Release | 13 | 0 | ⬜ Not started |
| **Total** | **123** | **0** | |

---

# Phase 0 — Specification Lock & Scaffolding (Plan §20 Phase 0, §19)

## Module 0.1 — Product Specification

| ID | Task | Plan Ref | Status | Updated | Notes |
|---|---|---|---|---|---|
| 0.1.1 | Final scope matrix: supported targets, default origin scope, cookie-scope exception, history scope, mandatory cannot-clear disclosure list | §3.1–3.5 | ⬜ | — | |
| 0.1.2 | Data-category matrix finalized with defaults, Quick Scrub and Advanced presets, per-option consequence statements | §4, §4.1, §4.2 | ⬜ | — | |
| 0.1.3 | Exact UI copy drafted in `_locales/en/messages.json` (all §24 strings incl. other-tabs notice, managed-browser message, SW-unselected note) | §14.8, §24, AUD-38 | ⬜ | — | |
| 0.1.4 | Permission model with recorded exclusion tradeoffs documented (`docs/PERMISSION_RATIONALE.md`) | §5.1–5.3, AUD-21, AUD-37 | ⬜ | — | |
| 0.1.5 | Initial threat model written (`docs/THREAT_MODEL.md`) covering AUD-21…AUD-45 controls and residual risks | §16, §2 | ⬜ | — | |
| 0.1.6 | Acceptance criteria document derived from the 18 release criteria | §23 | ⬜ | — | |
| 0.1.7 | **Exit gate:** no unresolved ambiguity on cookie scope, history scope, incognito, unsupported schemes, deletion order, other-tab limitation copy, partitioning disclosure | §20 Phase 0 | ⬜ | — | |

## Module 0.2 — Toolchain, Build & CI Scaffolding (AUD-30)

| ID | Task | Plan Ref | Status | Updated | Notes |
|---|---|---|---|---|---|
| 0.2.1 | Repository skeleton created per recommended directory structure | §7 | ⬜ | — | |
| 0.2.2 | `package.json`, TypeScript `strict: true`, `chrome-types` typings configured | §19.1 | ⬜ | — | |
| 0.2.3 | esbuild pipeline `tools/build.mjs` — deterministic output, dev/prod modes, PSL parser bundled at build time | §19.1 | ⬜ | — | |
| 0.2.4 | ESLint (incl. rule banning `innerHTML`/string-built HTML) + Prettier configured | §19.1 | ⬜ | — | |
| 0.2.5 | Vitest set up with typed `chrome.*` mock layer | §19.1 | ⬜ | — | |
| 0.2.6 | Playwright e2e harness launching Chromium with unpacked build loaded | §19.1 | ⬜ | — | |
| 0.2.7 | `tools/package-audit.mjs` skeleton (full checks completed in 5.3.2) | §18.7, §19.1 | ⬜ | — | |
| 0.2.8 | `tools/release.mjs` — store ZIP + checksum + changelog-entry gate | §19.1, §21 | ⬜ | — | |
| 0.2.9 | GitHub Actions `ci.yml`: install/typecheck/lint → unit → build → package audit → e2e matrix (min + stable Chrome) → artifact | §19.2 | ⬜ | — | |
| 0.2.10 | `manifest.json` blueprint: MV3, `__MSG__` i18n keys, `minimum_chrome_version: 120`, `incognito: not_allowed`, no `web_accessible_resources`, no `externally_connectable`, CSP | §5.4 | ⬜ | — | |

---

# Phase 1 — Core Target & Validation Layer (Plan §20 Phase 1)

## Module 1.1 — URL & Origin Safety (§10)

| ID | Task | Plan Ref | Status | Updated | Notes |
|---|---|---|---|---|---|
| 1.1.1 | `shared/url-normalizer` — canonicalization rules (lowercase scheme/host, default-port removal, terminal dot, IPv6, punycode handling) | §10.1 | ⬜ | — | |
| 1.1.2 | Allowed-scheme function — exact `http:`/`https:` only, no prefix checks | §10.2 | ⬜ | — | |
| 1.1.3 | `shared/origin-matcher` — exact-origin matcher with `try/catch` URL parse | §10.3 | ⬜ | — | |
| 1.1.4 | URL readability rule: empty/unset/unparsable `tab.url` at any revalidation point ⇒ `TARGET_CHANGED`, never continuity | §10.6, AUD-23 | ⬜ | — | |
| 1.1.5 | `shared/registrable-domain` — pinned, audited PSL parser bundled in `vendor/` with license; no runtime downloads | §10.4 | ⬜ | — | |
| 1.1.6 | Conservative fallback warning path when registrable domain can't be confidently calculated; IP/localhost handling | §10.4 | ⬜ | — | |
| 1.1.7 | Hostname boundary matcher (exact or `"." + targetHost` suffix; `includes()` prohibited) for future subdomain mode | §10.5 | ⬜ | — | |

## Module 1.2 — Messaging & Validation (§9)

| ID | Task | Plan Ref | Status | Updated | Notes |
|---|---|---|---|---|---|
| 1.2.1 | `shared/message-types` — allowlisted types: `ORIGINSCRUB_PREPARE`, `ORIGINSCRUB_EXECUTE`, `ORIGINSCRUB_STATUS` | §9.1–9.3 | ⬜ | — | |
| 1.2.2 | `shared/schemas` — strict payload validation; unknown keys rejected; finite positive-integer IDs; strict booleans | §9, §9.5 | ⬜ | — | |
| 1.2.3 | `shared/result-types` — per-category result shape incl. frame breakdown and history counts | §9.4 | ⬜ | — | |
| 1.2.4 | Sender validation in worker: `sender.id === chrome.runtime.id`, approved extension-page URL, no UI-supplied origin used as deletion authority | §9.5, AUD-01 | ⬜ | — | |
| 1.2.5 | External surface sealed: never register `onMessageExternal`/`onConnectExternal`; no `externally_connectable` key | §9.5, AUD-31 | ⬜ | — | |

## Module 1.3 — State, Tokens & Concurrency (§13)

| ID | Task | Plan Ref | Status | Updated | Notes |
|---|---|---|---|---|---|
| 1.3.1 | `operation-store` on `chrome.storage.session` — tokens, snapshots, progress, locks metadata only; quota discipline (never URL lists) | §13.1, AUD-43 | ⬜ | — | |
| 1.3.2 | Retention TTLs: preflight token 60 s, in-progress 15 min, completed summary 10 min, immediate URL removal on return/close | §13.2 | ⬜ | — | |
| 1.3.3 | Token manager — `crypto.randomUUID()` operation/request IDs and tokens, single-use, exact string comparison worker-side, consumption recorded before destructive calls | §13.3, AUD-35 | ⬜ | — | |
| 1.3.4 | Web Locks execution guard — `navigator.locks.request` with `ifAvailable`, token consume inside lock, `LOCK_TIMEOUT` ceiling for internal paths | §13.4, AUD-22 | ⬜ | — | |
| 1.3.5 | Preference schema versioning — `schemaVersion` field, ordered migration map, safe reset on unknown future version | §13.5, AUD-44 | ⬜ | — | |
| 1.3.6 | `target-resolver` service — worker-side independent target derivation from re-queried tab | §8.1.9, AUD-01 | ⬜ | — | |

## Module 1.4 — Phase-1 Unit Tests (§18.1)

| ID | Task | Plan Ref | Status | Updated | Notes |
|---|---|---|---|---|---|
| 1.4.1 | Canonicalization/matching suite: HTTP vs HTTPS, ports, `notexample.com`, `example.com.evil.test`, subdomains, uppercase, trailing dot, punycode/IDN, `co.uk`/`com.au`, private-registry fixtures, IPv4/IPv6, localhost multi-port, malformed URLs, internal/opaque schemes | §18.1 | ⬜ | — | |
| 1.4.2 | Empty/undefined URL handling tests per §10.6 | §18.1, AUD-23 | ⬜ | — | |
| 1.4.3 | Message schema rejection tests | §18.1 | ⬜ | — | |
| 1.4.4 | Token expiry and reuse tests | §18.1 | ⬜ | — | |
| 1.4.5 | Web-Lock serialization test: two concurrent EXECUTE calls ⇒ exactly one pipeline | §18.1, AUD-22 | ⬜ | — | |
| 1.4.6 | Preference schema migration path tests | §18.1, AUD-44 | ⬜ | — | |
| 1.4.7 | **Exit gate:** all URL, message, migration, and concurrency-attack unit tests pass | §20 Phase 1 | ⬜ | — | |

---

# Phase 2 — Scrub Engine (Plan §20 Phase 2)

## Module 2.1 — Category Cleaners (§11.1–11.6)

| ID | Task | Plan Ref | Status | Updated | Notes |
|---|---|---|---|---|---|
| 2.1.1 | `session-storage-cleaner` — `allFrames: true` injection, per-frame origin+result, cross-origin frames reported `crossOriginSkipped`, top-frame failure ⇒ `failed`, subframe failure ⇒ `partial` | §11.1, AUD-36 | ⬜ | — | |
| 2.1.2 | Site-storage cleaner — dedicated `browsingData.remove` with explicit `since: 0`, `originTypes: { unprotectedWeb: true }`, SW excluded from the group | §11.2, AUD-34 | ⬜ | — | |
| 2.1.3 | Cookies cleaner — dedicated call, `since: 0`; UI labeling shows registrable-domain impact, never "exact origin" | §11.3 | ⬜ | — | |
| 2.1.4 | Service-worker cleaner — `{ serviceWorkers: true }` exact origin, off by default | §11.4 | ⬜ | — | |
| 2.1.5 | Network-cache cleaner — `{ cache: true }` origin-filtered, off by default | §11.5 | ⬜ | — | |
| 2.1.6 | Category isolation — one API call per category, never combined | §11.6 | ⬜ | — | |

## Module 2.2 — Pipeline & Tab Transition

| ID | Task | Plan Ref | Status | Updated | Notes |
|---|---|---|---|---|---|
| 2.2.1 | `tab-transition-service` — navigate to `scrubbing.html#<operation-id>`, await readiness ping; failure ⇒ `INTERSTITIAL_LOAD_FAILED` and abort before persistent deletion | §8.3.6, §15 | ⬜ | — | |
| 2.2.2 | Fixed-order pipeline in `operation-controller`: sessionStorage → interstitial → SW → siteStorage → networkCache → cookies → history | §11.7, AUD-27 | ⬜ | — | |
| 2.2.3 | Per-category progress persisted to `storage.session` after each category | §8.3.7 | ⬜ | — | |
| 2.2.4 | Per-category result aggregator (success/partial/failed/skipped, never one monolithic error) | §9.4, AUD-09/15 | ⬜ | — | |
| 2.2.5 | Return-navigation hardening — re-parse + scheme allowlist before `tabs.update`; fresh navigation only, never `history.back()`; `RETURN_NAVIGATION_FAILED` with copyable URL | §11.8, AUD-25/33 | ⬜ | — | |
| 2.2.6 | `verification-service` — post-deletion verification where verifiable; "verified" badge only where verification occurred | §11.9 | ⬜ | — | |

## Module 2.3 — Phase-2 Tests

| ID | Task | Plan Ref | Status | Updated | Notes |
|---|---|---|---|---|---|
| 2.3.1 | Integration fixture sites: cookies at host/parent scope, CHIPS partitioned cookie + third-party-embedded storage under second top-level fixture, Local/Session storage (top + same-origin + cross-origin iframe), IndexedDB, CacheStorage, SW, cacheable assets, multi-path history | §18.2, AUD-24 | ⬜ | — | |
| 2.3.2 | Isolation cases: scrub Site A keeps Site B signed in; parent-domain cookie disclosure accurate; hostile lookalike domains untouched | §18.3.1–3 | ⬜ | — | |
| 2.3.3 | Partitioned-data fixture outcome recorded in THREAT_MODEL.md and asserted against disclosure text | §18.3.4, AUD-24 | ⬜ | — | |
| 2.3.4 | Deletion-order invariant unit test | §18.1, §11.7 | ⬜ | — | |
| 2.3.5 | **Exit gate:** Site A scrubbed without altering Site B; partitioned-data outcome recorded and matched to disclosure copy | §20 Phase 2 | ⬜ | — | |

---

# Phase 3 — Optional History Module (Plan §20 Phase 3, §12)

## Module 3.1 — History Cleaner

| ID | Task | Plan Ref | Status | Updated | Notes |
|---|---|---|---|---|---|
| 3.1.1 | Permission gate — `permissions.contains({history})` checked at preflight **and re-checked at execution** | §12.1 | ⬜ | — | |
| 3.1.2 | Chunked scan — 5,000-result batch ceiling, empty free-text query, recursive time-range halving | §12.2.1–3 | ⬜ | — | |
| 3.1.3 | De-duplication by exact URL + exact-origin filtering of candidates | §12.2.4 | ⬜ | — | |
| 3.1.4 | Defensive limits — max recursion depth, max candidates, max duration; breach ⇒ `HISTORY_SCAN_LIMIT_REACHED` | §12.3 | ⬜ | — | |
| 3.1.5 | Controlled parallel batch deletion (10–25 promises); failed URLs in transient worker memory only | §12.2.6–7 | ⬜ | — | |
| 3.1.6 | Execution-time re-scan from scratch — preflight result display-only and discarded; actual deleted count reported, divergence stated plainly | §12.5, AUD-28 | ⬜ | — | |
| 3.1.7 | Verification recount after deletion; nonzero remainder ⇒ `partial` | §11.9, §12.5.4 | ⬜ | — | |
| 3.1.8 | Policy-block detection — restriction-indicating rejection or zero-delete-with-remainder ⇒ `POLICY_BLOCKED` with managed-browser copy | §12.6, AUD-29 | ⬜ | — | |
| 3.1.9 | Preflight count integration — "approximately — recounted at execution" labeling; URLs-removed (not visits) reporting | §8.2, §12.4 | ⬜ | — | |

## Module 3.2 — Phase-3 Tests

| ID | Task | Plan Ref | Status | Updated | Notes |
|---|---|---|---|---|---|
| 3.2.1 | Chunk splitting and de-duplication unit tests | §18.1 | ⬜ | — | |
| 3.2.2 | Similarly-named-domain non-deletion tests (automated + manual) | §20 Phase 3 gate | ⬜ | — | |
| 3.2.3 | Policy-block simulation ⇒ `POLICY_BLOCKED`, never false success | §18.4, AUD-29 | ⬜ | — | |
| 3.2.4 | History permission denied / revoked-between-phases tests | §18.4, §12.1 | ⬜ | — | |
| 3.2.5 | **Exit gate:** no similarly named domain deleted; policy-block yields `POLICY_BLOCKED` | §20 Phase 3 | ⬜ | — | |

---

# Phase 4 — Popup & Interstitial UX (Plan §20 Phase 4, §14)

## Module 4.1 — Popup

| ID | Task | Plan Ref | Status | Updated | Notes |
|---|---|---|---|---|---|
| 4.1.1 | Popup layout: logo, origin card, cookie-impact note, always-visible other-tabs notice, Quick Scrub, Advanced disclosure, Review scrub button, `aria-live` status area | §14.1, AUD-21 | ⬜ | — | |
| 4.1.2 | Quick/Advanced selections with consequence helper text per destructive option | §4.1–4.2, §14.1 | ⬜ | — | |
| 4.1.3 | Preflight flow: scheme validation, PREPARE round-trip, safe-summary rendering; cookie scope-not-counts helper text | §8.1, AUD-37 | ⬜ | — | |
| 4.1.4 | Confirmation screen with §14.2 concrete language, history "approximately" label, SW warnings, cannot-undo statement, Scrub this origin / Cancel | §8.2, §14.2 | ⬜ | — | |
| 4.1.5 | Optional history permission flow from user gesture; `pendingPermissionRequest` persisted before native prompt; denial unselects history and explains continuation | §8.1.6, AUD-26 | ⬜ | — | |
| 4.1.6 | Popup state persistence — every selection change written to `storage.session` keyed by tab ID; restore on open; expiry with token TTL; cleared on completion/cancel | §14.6, AUD-26 | ⬜ | — | |
| 4.1.7 | Double-submit UI guards: disable controls post-confirm, ignore duplicate request IDs, reject second active op per tab (convenience layer over Web Lock) | §14.5 | ⬜ | — | |

## Module 4.2 — Scrubbing Interstitial

| ID | Task | Plan Ref | Status | Updated | Notes |
|---|---|---|---|---|---|
| 4.2.1 | Interstitial states: Preparing → per-category progress (mirrors §11.7 order) → Complete/Partial/Failed/Interrupted | §14.3 | ⬜ | — | |
| 4.2.2 | Interstitial hardening — validate `#<operation-id>` against `storage.session`; unknown/absent/expired ⇒ neutral "No active operation" with zero target info; not web-accessible | §14.7, AUD-32 | ⬜ | — | |
| 4.2.3 | Live progress via `storage.onChanged` + `ORIGINSCRUB_STATUS` re-request after worker restart | §13.6, §9.3 | ⬜ | — | |
| 4.2.4 | Results UI: per-category outcomes, verified badges only where verified, retry-failed-items only when safe | §8.3.10, §11.9 | ⬜ | — | |
| 4.2.5 | Return to site / Close tab controls with URL revalidation and fresh navigation | §11.8 | ⬜ | — | |

## Module 4.3 — Onboarding, i18n & Accessibility

| ID | Task | Plan Ref | Status | Updated | Notes |
|---|---|---|---|---|---|
| 4.3.1 | Onboarding page shown once on install (first-run flag in `storage.local`): what it does, §3.5 cannot-do list verbatim, other-tabs limitation illustrated, prefs location, single "Got it" | §14.9, AUD-39 | ⬜ | — | |
| 4.3.2 | i18n plumbing — every user-visible string through `chrome.i18n.getMessage` / `__MSG__`; placeholders only, no concatenation assuming English word order | §14.8, AUD-38 | ⬜ | — | |
| 4.3.3 | RTL-safe CSS using logical properties throughout | §14.8 | ⬜ | — | |
| 4.3.4 | Accessibility: native controls, visible focus, no color-only info, `aria-live` progress/results, target sizes, zoom without clipping, `prefers-reduced-motion` | §14.4 | ⬜ | — | |
| 4.3.5 | Error taxonomy surfaced in UI — all §15 codes mapped to user-facing copy; no raw stack traces | §15 | ⬜ | — | |

## Module 4.4 — Phase-4 Tests

| ID | Task | Plan Ref | Status | Updated | Notes |
|---|---|---|---|---|---|
| 4.4.1 | Accessibility tests: keyboard-only workflow, screen-reader labels, 200% zoom, high contrast, reduced motion, error announcements | §18.6 | ⬜ | — | |
| 4.4.2 | Popup-close-during-permission-prompt cycle test with state restoration | §18.4, AUD-26 | ⬜ | — | |
| 4.4.3 | **Exit gate:** complete keyboard and screen-reader workflow; popup survives permission-prompt close/reopen with state intact | §20 Phase 4 | ⬜ | — | |

---

# Phase 5 — Security & Recovery (Plan §20 Phase 5)

## Module 5.1 — Race, Recovery & Resurrection Tests (§18.4)

| ID | Task | Plan Ref | Status | Updated | Notes |
|---|---|---|---|---|---|
| 5.1.1 | Navigation races: navigate between preflight↔confirmation and confirmation↔execution ⇒ clean abort | §18.4 | ⬜ | — | |
| 5.1.2 | Cross-origin navigation making `tab.url` unreadable ⇒ `TARGET_CHANGED`, not an exception | §18.4, AUD-23 | ⬜ | — | |
| 5.1.3 | Tab closed during deletion; popup closed immediately after execution | §18.4 | ⬜ | — | |
| 5.1.4 | Service worker terminated/reloaded mid-operation ⇒ `INTERRUPTED` recovery with safe-retry-only categories | §18.4, §13.6 | ⬜ | — | |
| 5.1.5 | Browser restart mid-operation ⇒ `STATE_LOST` neutral view, no target details | §18.4, AUD-43 | ⬜ | — | |
| 5.1.6 | Site A open in second tab during scrub — resurrection outcome documented; warning copy asserted | §18.4, AUD-21 | ⬜ | — | |
| 5.1.7 | Back button after scrub — BFCache behavior recorded; disclosure accuracy asserted | §18.4, AUD-25 | ⬜ | — | |
| 5.1.8 | Single-category failure simulation ⇒ accurate partial reporting, remaining categories continue | §18.4, §15 | ⬜ | — | |

## Module 5.2 — Security Hardening & Tests

| ID | Task | Plan Ref | Status | Updated | Notes |
|---|---|---|---|---|---|
| 5.2.1 | Two EXECUTE messages fired back-to-back programmatically ⇒ exactly one pipeline runs (e2e) | §18.4, AUD-22 | ⬜ | — | |
| 5.2.2 | External-message rejection test from a second test extension — never observed by OriginScrub handlers | §9.5, AUD-31 | ⬜ | — | |
| 5.2.3 | Interstitial opened directly / with unknown operation ID ⇒ neutral state, no enumeration | §14.7, AUD-32 | ⬜ | — | |
| 5.2.4 | Extension-page XSS defenses verified: no inline scripts, no remote code, no `innerHTML` for untrusted values (lint rule + audit) | §16.1 | ⬜ | — | |
| 5.2.5 | Sanitized diagnostics; development-only debug mode; no stack traces to users | §15, §20 Phase 5 | ⬜ | — | |

## Module 5.3 — Performance & Package Audit

| ID | Task | Plan Ref | Status | Updated | Notes |
|---|---|---|---|---|---|
| 5.3.1 | Performance matrix: empty profile, large IndexedDB, PWA+SW+CacheStorage, 100/5,000/>5,000 history URLs, huge unrelated history; record durations, memory (§13.1 quota discipline), failure rate | §18.5 | ⬜ | — | |
| 5.3.2 | Package audit fully implemented and failing CI on violation: manifest valid, no remote scripts, no runtime downloads, licenses present, no inline script, no unexpected host permissions, no `web_accessible_resources`, no `externally_connectable`/external listeners, no dev secrets, no source-map leaks, all strings i18n-resolved, icons/screenshots complete | §18.7, §19.1 | ⬜ | — | |

## Module 5.4 — Threat-Model Closure

| ID | Task | Plan Ref | Status | Updated | Notes |
|---|---|---|---|---|---|
| 5.4.1 | Threat-model checklist complete: every AUD-21…AUD-45 item marked resolved or formally accepted-and-disclosed | §20 Phase 5 | ⬜ | — | |
| 5.4.2 | **Exit gate:** no open critical or high findings | §20 Phase 5 | ⬜ | — | |

---

# Phase 6 — Store Compliance & Release (Plan §20 Phase 6, §17, §21)

## Module 6.1 — Store Materials

| ID | Task | Plan Ref | Status | Updated | Notes |
|---|---|---|---|---|---|
| 6.1.1 | Privacy policy (`docs/PRIVACY_POLICY.md`) — local processing, no transmission, retention, sync-propagation disclosure, uninstall, limited-use statement | §17.2, AUD-41 | ⬜ | — | |
| 6.1.2 | Permission justification finalized for store submission | §17.1, §5.2 | ⬜ | — | |
| 6.1.3 | Store listing (`docs/STORE_LISTING.md`) + screenshots (preflight + confirmation) + data-usage form ("does not collect or transmit user data") | §17.1, §24 | ⬜ | — | |
| 6.1.4 | Reviewer guide: test URL, invocation steps, optional-permission explanation, internal scrubbing page explanation, no-remote-requests confirmation, source-map policy, production build instructions | §17.3 | ⬜ | — | |
| 6.1.5 | Edge delta (`docs/EDGE_LISTING_DELTA.md`) — Partner Center listing metadata, screenshots, review notes, version-parity policy | §17.4, AUD-45 | ⬜ | — | |
| 6.1.6 | `docs/RELEASE_CHECKLIST.md` — sign-off items incl. minimum-Chrome-version rationale record | §5.4, §19.2 | ⬜ | — | |

## Module 6.2 — Release Engineering

| ID | Task | Plan Ref | Status | Updated | Notes |
|---|---|---|---|---|---|
| 6.2.1 | Production release ZIP + checksum via `tools/release.mjs`; changelog entry gate satisfied | §19.1, §21 | ⬜ | — | |
| 6.2.2 | Staged-rollout plan documented (percentage rollout, hold period, monitoring cadence, expedited-patch rollback path, previous-ZIP archival) | §21 | ⬜ | — | |
| 6.2.3 | Edge manual test pass on current stable Edge, scripted | §18.8, §17.4 | ⬜ | — | |
| 6.2.4 | Full browser matrix green: pinned minimum Chrome + current stable Chrome automated, Edge manual | §18.8 | ⬜ | — | |

## Module 6.3 — Final Verification

| ID | Task | Plan Ref | Status | Updated | Notes |
|---|---|---|---|---|---|
| 6.3.1 | §25 official-reference verification — every load-bearing item (activeTab semantics, Web Locks in workers, partitioning/CHIPS, BFCache, store policies) gets a dated note in THREAT_MODEL.md | §25 | ⬜ | — | |
| 6.3.2 | All 18 release acceptance criteria verified and checked off | §23 | ⬜ | — | |
| 6.3.3 | **Exit gate:** clean production package passes automated package audit + manual reviewer walkthrough on Chrome and Edge | §20 Phase 6 | ⬜ | — | |
