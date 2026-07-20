# Evaluation — Kanban Board · Senior

## Specs

- [ ] Core holds: API CRUD, URL-synced label filter, auth, optimistic drag, card modal route
- [ ] Query cache with dedup + invalidation; can explain which views a mutation refreshes and why
- [ ] Stale responses cancelled — fast board/filter switching never renders an out-of-order result
- [ ] Another tab's move/edit shows without a reload (storage / BroadcastChannel / polling / WS)
- [ ] An in-flight drag shows as pending in the other tab rather than jumping under the cursor
- [ ] Conflict resolution when two clients move the same card — no lost/duplicated cards under a race
- [ ] Stale-version move refused and resolved, not silently overwritten
- [ ] Retry with backoff on 5xx; no double-apply; 4xx surfaced immediately
- [ ] Offline moves queue, show as pending, replay in order
- [ ] Roles owner/member enforced in the API (a member can't delete the board, even if forced)
- [ ] Board sharing is owner-only — a member can't promote themselves, however the request is crafted
- [ ] Bulk move/delete, optimistic, rollback names the failed cards
- [ ] Undo applies a real inverse through the API and is audited
- [ ] ~5,000-card board virtualized — drags at frame rate, dnd-kit still works with windowed rows
- [ ] Full keyboard drag + screen-reader announcements; managed focus in menus/dialogs
- [ ] Zero axe violations, asserted in a test; live regions announce async outcomes
- [ ] Audit log written inside the API layer — a scripted request is logged too; before → after captured
- [ ] Playwright covers the full matrix (two-tab sync, card by URL, 403, partial failure, conflict, keyboard)
- [ ] Deployed; link works

## Code quality

- [ ] Strict TypeScript
- [ ] Authorization **and** auditing in the API layer, not the component
- [ ] Before/after perf numbers written down, with the method
- [ ] Memoization justified by a profile
- [ ] Real unit + integration tests (reorder, conflict, authorization, cache)

## Workflow

- [ ] Talked through the flow before diving in
- [ ] Weighs trade-offs (conflict strategy, sync channel, a11y) on their own
- [ ] Can explain any line
- [ ] Bonus: did a Stretch item

## Verdict

- [ ] Right at Senior
- [ ] Below Senior
