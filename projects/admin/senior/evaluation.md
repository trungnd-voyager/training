# Evaluation — Admin Dashboard · Senior

## Specs

- [ ] URL-synced tables + optimistic CRUD (the Middle baseline still holds)
- [ ] Roles enforced in the API — a `viewer`'s write is refused (`403`) even when forced; UI adapts
- [ ] Advanced query operators (ranges, multi-value OR) that compose with sort + paging
- [ ] 10k rows virtualized — smooth, no 10k DOM nodes
- [ ] Bulk actions, optimistic, roll back on partial failure
- [ ] Polling for freshness; activity/audit log
- [ ] Keyboard + focus handling in dialogs
- [ ] At least one Playwright E2E

## Code quality

- [ ] Strict TypeScript
- [ ] Authorization in the data layer, not the component
- [ ] Performance actually measured under 10k rows
- [ ] Real unit + integration tests

## Workflow

- [ ] Talked through the flow before diving in
- [ ] Weighs trade-offs (conflicts, freshness, a11y) on their own
- [ ] Can explain any line
- [ ] Bonus: did a Stretch item

## Verdict

- [ ] Right at Senior
- [ ] Below Senior
