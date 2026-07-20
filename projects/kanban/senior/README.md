# Project — Kanban Board

Build a production-grade collaborative task board as a **client-side single-page
app** — real-time collaboration, conflict handling, roles enforced in the API layer, accessible
drag-and-drop, and a real test suite. No real backend. This is your project for the month.

## Setup

- **Stack:** React + Vite + **TypeScript**, with **React Router**.
- **Drag-and-drop:** **dnd-kit** (its keyboard sensor matters for the accessibility requirement).
- **Data:** a **mock API layer** over an in-memory + **localStorage** dataset (async, latency); cards stored
  **normalized**. **[MSW](https://mswjs.io/)** recommended. The shared localStorage store lets multiple tabs
  sync via a **real-time channel** — a `storage` event, `BroadcastChannel`, or polling; a small WebSocket
  mock is a stretch
  ([**`@faker-js/faker`**](https://www.npmjs.com/package/@faker-js/faker) recommended for seed data). Seed
  one **stress board with ~5,000 cards** to exercise performance. Mock users carry a board `role` of
  `owner` or `member`.
- **Testing:** Vitest + React Testing Library + **Playwright** (E2E).
- **Responsive:** the board works down to mobile.

## Screens & routes

| Route                     | Screen       | Purpose                                     |
| ------------------------- | ------------ | ------------------------------------------- |
| `/login`                  | Login        | Session sign-in                             |
| `/`                       | Boards list  | The user's boards; create / rename / delete |
| `/board/:id`              | Board view   | Live-updating; keyboard-draggable cards     |
| `/board/:id/card/:cardId` | Card detail  | Modal route; full card page on direct visit |
| `/board/:id/activity`     | Activity log | Audit feed (who / what / when)              |

## Feature specs

### Core

- [ ] API-persisted boards/columns/cards; column & card CRUD; URL-synced label filter; auth + guard.
- [ ] Drag-and-drop with **optimistic** updates that **roll back on error**.
- [ ] Card detail as a **modal route** (shareable URL; full-page fallback on direct visit).
- [ ] A **query cache** — in-flight dedup, stale-while-revalidate, and explicit invalidation on mutation.
      Use TanStack Query or write your own; either way be able to explain why a mutation refreshes exactly
      the views it should.
- [ ] **Bulk actions:** multi-select cards, then move or delete them in one go — optimistic, with
      **rollback on partial failure** and a result that names which cards failed and why.
- [ ] **Undo** on destructive and bulk actions within a short window, applied as a real inverse operation
      through the API layer and itself recorded in the audit log.
- [ ] **Performance at scale:** the ~5,000-card board stays smooth. Columns **virtualize** their cards
      (e.g. `@tanstack/react-virtual`) and dragging stays at frame rate — dnd-kit must keep working with
      windowed rows, including auto-scroll near a column edge.
- [ ] Measure it: record drag/scroll FPS before and after virtualization, and put the two numbers plus
      your method in the repo README. Justify one memoization decision with a profile.

### Real-time collaboration

- [ ] Another tab's move/edit appears **without a full reload** — via a `storage` event, `BroadcastChannel`,
      polling, or a WebSocket mock. State a choice and make it work.
- [ ] A card being dragged in one tab shows as **in-flight** in the other, rather than jumping under the
      user's cursor when the move lands.
- [ ] **Offline queue:** moves and edits made while offline are queued, surfaced as pending in the UI, and
      replayed in order on reconnect — with conflicts and rejections resolved, not dropped.

### Concurrency & conflicts

- [ ] Define and implement a resolution for two clients moving the **same card** at once (e.g.
      last-write-wins with reconciliation, or positional merge). No lost or duplicated cards under a race.
- [ ] Cards carry a **version**; a move or edit against a stale version is rejected by the API layer and
      resolved in the UI — never a silent overwrite.
- [ ] Distinguish transient from permanent failures: retry 5xx with backoff, surface 4xx immediately.
      A retried move must not apply twice.
- [ ] **Request cancellation:** switching boards or filters fast fires overlapping requests; a slow
      earlier response must never overwrite a newer one. Demonstrate with staggered latency.

### Roles — enforced in the API layer

- [ ] Board `owner` vs `member`. The **API layer authorizes every action** — a `member` cannot delete the
      board even if the request is forced past the UI. The UI also adapts to role.
- [ ] A board can be **shared** with another user at a chosen role, and the share itself is owner-only —
      a `member` cannot promote themselves, however the request is crafted.

### Accessibility

- [ ] **Full keyboard drag-and-drop** (pick up, move, drop) via dnd-kit's keyboard sensor, with
      screen-reader announcements. Menus/dialogs are keyboard-navigable with managed focus, and focus
      returns to the trigger on close.
- [ ] Live regions announce async outcomes (move landed, bulk result, rollback) as well as drag state.
- [ ] **Zero axe violations** on every route, asserted in a test — not a manual pass.

### Auditability & testing

- [ ] An **activity log** — append-only, written **inside the API layer** so a forced or scripted request
      is logged too, recording actor / action / card / before → after / timestamp. Filterable by actor.
- [ ] Meaningful unit + integration tests (reorder, conflict resolution, authorization, the cache layer).
- [ ] **Playwright E2E covering:** a move in one tab syncing to a second; opening a card by URL; a
      `member`'s forced owner-only action getting `403`; a bulk move with a seeded partial failure; a
      stale-version conflict and its resolution; a card moved end-to-end by keyboard only.

## Deploy

- [ ] Deployed to Vercel or Netlify; the live link is in the repo README.
- [ ] The production build runs clean — no type errors, no console warnings on the deployed app.

## Done check

The E2E suite passes; two tabs stay in sync and an in-flight drag doesn't jump under the other user's
cursor; a concurrent move on the same card resolves without corruption and a stale version is refused; a
`member` is refused by the API layer on owner-only actions and can't promote themselves; the ~5,000-card
board drags at frame rate and the before/after numbers are written down; a bulk move names exactly which
cards failed; undo restores through the API and is audited; moves made offline replay in order on
reconnect; a card can be moved entirely by keyboard; axe is clean on every route.

**Stretch (optional):** assignees + @mentions; card comments; a WebSocket mock in place of polling.
