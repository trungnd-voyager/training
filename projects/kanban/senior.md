# Project — Kanban Board

Build a production-grade collaborative task board as a **client-side single-page
app** — real-time collaboration, conflict handling, roles enforced in the API layer, accessible
drag-and-drop, and a real test suite. No real backend. This is your project for the month.

## Setup

- **Stack:** React + Vite + **TypeScript**, with **React Router**. Avoid `any`.
- **Drag-and-drop:** **dnd-kit** (its keyboard sensor matters for the accessibility requirement).
- **Data:** a **mock API layer** persisting boards/columns/cards (normalized) to localStorage, plus a
  **real-time channel** so multiple tabs sync — a `BroadcastChannel` or polling is fine; a small WebSocket
  mock is a stretch ([**`@faker-js/faker`**](https://www.npmjs.com/package/@faker-js/faker) recommended for
  seed data). Mock users carry a board `role` of `owner` or `member`.
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

- [ ] API-persisted boards/columns/cards; column & card CRUD; label filter; auth + guard.
- [ ] Drag-and-drop with **optimistic** updates that **roll back on error**.
- [ ] Card detail as a **modal route** (shareable URL; full-page fallback on direct visit).

### Real-time collaboration

- [ ] Another tab's move/edit appears **without a full reload** — via `BroadcastChannel`, polling, or a
      WebSocket mock. State a choice and make it work.

### Concurrency & conflicts

- [ ] Define and implement a resolution for two clients moving the **same card** at once (e.g.
      last-write-wins with reconciliation, or positional merge). No lost or duplicated cards under a race.

### Roles — enforced in the API layer

- [ ] Board `owner` vs `member`. The **API layer authorizes every action** — a `member` cannot delete the
      board even if the request is forced past the UI. The UI also adapts to role.

### Accessibility

- [ ] **Full keyboard drag-and-drop** (pick up, move, drop) via dnd-kit's keyboard sensor, with
      screen-reader announcements. Menus/dialogs are keyboard-navigable with managed focus.

### Auditability & testing

- [ ] An **activity log** records moves/edits (who, what, when) on `/board/:id/activity`.
- [ ] Meaningful unit + integration tests (reorder, conflict resolution, authorization).
- [ ] **At least one Playwright E2E:** move a card, reload, order persisted; open a card via its URL.

## Done check

Two tabs stay in sync; a concurrent move on the same card resolves without corruption; a `member` is refused
by the API layer on owner-only actions; a card can be moved entirely by keyboard; the E2E passes.

**Stretch (optional):** assignees + @mentions; card comments; per-board member invites.
