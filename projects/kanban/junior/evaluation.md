# Evaluation — Kanban Board · Junior

## Specs

- [ ] Mock login + guard (logged-out → `/login`); logout clears the session
- [ ] Boards list: create / rename / delete; a board belongs to its owner; delete leaves nothing orphaned
- [ ] Board view: columns left→right, card count, empty-column placeholder
- [ ] Card shows title, description hint, labels, due date
- [ ] Columns: add / rename inline / delete (confirm)
- [ ] Columns reorder by drag; `columnOrder` drives the order and survives reload
- [ ] Inline add card; card modal at its URL (modal over board; direct visit = full page)
- [ ] Edit title/desc/labels/due; delete confirms; filter by label
- [ ] Drag within + across columns — immutable, no lost/duplicated cards
- [ ] Activity log; survives reload; no console warnings
- [ ] Tests for reorder/move + label filter, and they pass
- [ ] Deployed; link works

## Code quality

- [ ] Strict TypeScript
- [ ] Normalized state updated immutably
- [ ] Sensible components, not much repetition
- [ ] Tests assert behaviour, not implementation detail

## Workflow

- [ ] Talked through the flow before diving in
- [ ] Asks good questions instead of guessing
- [ ] Can explain any line
- [ ] Bonus: did a Stretch item

## Verdict

- [ ] Right at Junior
- [ ] Ahead
- [ ] Not there yet
