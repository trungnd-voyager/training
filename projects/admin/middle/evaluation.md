# Evaluation — Admin Dashboard · Middle

## Specs

- [ ] Login + guard on the whole admin; logout clears it; return-to-intended-route works
- [ ] Tables read through the API layer, not the store
- [ ] Filter/sort/page live in the URL (shareable, refresh-safe, back works)
- [ ] CRUD via the API — optimistic, rolls back on failure
- [ ] Products **and** customers CRUD, sharing plumbing rather than duplicated
- [ ] Failure injection exists; rollback demonstrable without a code edit
- [ ] Dashboard cards + chart; sections load on their own
- [ ] Loading per route, error boundary, not-found
- [ ] Tests for the table logic + optimistic success and rollback
- [ ] Deployed; link works

## Code quality

- [ ] Clean API boundary
- [ ] Strict TypeScript
- [ ] Optimistic + rollback without corrupting state
- [ ] Second resource reuses the shared abstraction rather than duplicating it

## Workflow

- [ ] Talked through the flow before diving in
- [ ] Can explain the client/server boundary and why it matters
- [ ] Can explain any line
- [ ] Bonus: did a Stretch item

## Verdict

- [ ] Right at Middle
- [ ] Ahead
- [ ] Not there yet
