# Project — Kanban Board

Build a task board as a **client-side single-page app** — boards, an API layer for
persistence, optimistic drag-and-drop, a modal route, and auth. No real backend. This is your project for
the month.

## Setup

- **Stack:** React + Vite + **TypeScript**, with **React Router**. Avoid `any`.
- **Drag-and-drop:** **dnd-kit** recommended.
- **Data:** a **mock API layer** (async, latency) persisting boards/columns/cards to localStorage so they
  survive reload; the UI talks to the layer, **not the raw store**. Store cards **normalized** — cards carry
  `labels` and an optional `dueDate`, boards an `ownerId`
  ([**`@faker-js/faker`**](https://www.npmjs.com/package/@faker-js/faker) recommended for seed data). Plus
  mock users for login.
- **Testing:** Vitest + React Testing Library.
- **Responsive:** the board works down to mobile.

## Screens & routes

| Route                     | Screen       | Purpose                                                    |
| ------------------------- | ------------ | ---------------------------------------------------------- |
| `/login`                  | Login        | Session sign-in                                            |
| `/`                       | Boards list  | The user's boards; create / rename / delete                |
| `/board/:id`              | Board view   | Columns + cards; drag to move/reorder                      |
| `/board/:id/card/:cardId` | Card detail  | Modal route over the board; full card page on direct visit |
| `/board/:id/activity`     | Activity log | Feed of moves & edits                                      |

## Feature specs

### Auth + boards

- [ ] Login (mock users) sets a session; the app is protected by a **route guard** — logged-out → `/login`.
- [ ] `/` lists the user's boards; a board belongs to its owner; create / rename / delete.

### Board view

- [ ] Board, columns, and cards load from the **mock API layer**; add / rename / delete columns.
- [ ] Cards show labels + due date; **filter** a board's cards by label.

### Card detail as a modal route

- [ ] Clicking a card opens a **modal** at `/board/:id/card/:cardId` (shareable URL); a **refresh or direct
      visit shows the full card page** (React Router background-location or an overlay layout).
- [ ] Edit title, description, labels, due date; save through the API layer.

### Drag-and-drop with optimistic persistence

- [ ] Reorder within a column and move across columns updates the UI **optimistically**, then persists via
      the API layer; on failure it **rolls back**.
- [ ] A failed save never corrupts order (no lost/duplicated cards).

### App concerns

- [ ] Route-level loading/error states + a not-found route; an activity feed on `/board/:id/activity`.
- [ ] Tests: unit tests for the reorder/move logic + a couple of component tests.

## Done check

Drag persists optimistically and rolls back on a forced failure; the card modal is shareable and a refresh
shows the full page; boards are per-user and protected; tests pass.

**Stretch (optional):** card comments/activity; assignees.
