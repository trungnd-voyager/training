# Evaluation — Admin Dashboard · Senior

## Specs

- [ ] URL-synced tables + optimistic CRUD (the Middle baseline still holds)
- [ ] Roles enforced in the API — a `viewer`'s write is refused (`403`) even when forced; UI adapts
- [ ] Restricted column stripped by the API, not just hidden in the UI
- [ ] Advanced query operators (ranges, multi-value OR) that compose with sort + paging; a garbage URL
      errors cleanly
- [ ] Stale responses cancelled — fast typing never renders an out-of-order result
- [ ] Query cache with dedup + invalidation; can explain which views a mutation refreshes and why
- [ ] 10k rows virtualized — smooth, no 10k DOM nodes; filter/sort re-queries the API, not memory
- [ ] Virtualized table keyboard-navigates with focus surviving row unmount; sticky header; variable rows
- [ ] Bulk actions incl. select-all-matching; rollback names the failed items
- [ ] Undo applies a real inverse through the API and is audited
- [ ] Stale-version save refused (`409`) and resolved, not silently overwritten
- [ ] Cross-tab freshness, with the polling-vs-broadcast trade-off argued
- [ ] Audit log written inside the API layer — a scripted request is logged too; before → after captured
- [ ] Retry with backoff on 5xx; no double-apply; 4xx surfaced immediately
- [ ] Offline mutations queue, show as pending, replay in order
- [ ] Zero axe violations, asserted in a test; live regions announce async outcomes
- [ ] Playwright covers the full matrix (403, partial failure, 409, keyboard path)
- [ ] Deployed; link works

## Code quality

- [ ] Strict TypeScript
- [ ] Authorization **and** auditing in the data layer, not the component
- [ ] Before/after perf numbers written down, with the method
- [ ] Memoization justified by a profile
- [ ] Unit + integration tests

## Workflow

- [ ] Talked through the flow before diving in
- [ ] Weighs trade-offs (conflicts, freshness, a11y) on their own
- [ ] Can explain any line
- [ ] Bonus: did a Stretch item

## Verdict

- [ ] Right at Senior
- [ ] Below Senior
