# Evaluation — Kanban Board · Middle

## Specs

- [ ] Login session + guard; logout clears it; return-to-intended-route works
- [ ] Boards / columns / cards load from the API layer; column CRUD + column reorder
- [ ] Another user's board id is refused by the API, not just hidden from the list
- [ ] Cards show labels + due; label filter URL-synced (shareable, refresh-safe, back works)
- [ ] Card modal route: shareable URL, full-page fallback on refresh/direct
- [ ] Every mutation optimistic through one shared path — not just drag; rolls back on failure
- [ ] Failure injection exists; rollback demonstrable without a code edit
- [ ] Route loading/error + not-found; activity feed
- [ ] Tests for the reorder/move logic + optimistic success and rollback
- [ ] Deployed; link works

## Code quality

- [ ] Strict TypeScript
- [ ] Clean API boundary — UI talks to the layer, not the store
- [ ] Reorder/move stays immutable (no lost/duplicated cards)
- [ ] One mutation path reused across resources rather than duplicated per action

## Workflow

- [ ] Talked through the flow before diving in
- [ ] Can explain the modal route + full-page fallback
- [ ] Can explain any line
- [ ] Bonus: did a Stretch item

## Verdict

- [ ] Right at Middle
- [ ] Ahead
- [ ] Not there yet
