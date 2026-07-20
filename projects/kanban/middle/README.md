# Project — Kanban Board

Build a task board as a **client-side single-page app** — boards, an API layer,
optimistic drag-and-drop, a modal route, and auth. No real backend. This is your project for
the month.

## Setup

- **Stack:** React + Vite + **TypeScript**, with **React Router**.
- **Drag-and-drop:** **dnd-kit** recommended.
- **Data:** a **mock API layer** over an in-memory + **localStorage** dataset (async, latency).
  **[MSW (Mock Service Worker)](https://mswjs.io/)** recommended. Store cards **normalized** — cards carry
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

- [ ] Login (mock users) sets a session; the app is protected by a **route guard** — logged-out → `/login`,
      returning to the route they asked for after signing in.
- [ ] Logout clears the session; a stale session is rejected by the API layer, not just the UI.
- [ ] `/` lists the user's boards; a board belongs to its owner; create / rename / delete. Another user's
      board id is refused by the API, not merely hidden from the list.

### Board view

- [ ] Board, columns, and cards load from the **mock API layer**; add / rename / delete columns, and
      reorder columns by drag.
- [ ] Cards show labels + due date; **filter** a board's cards by label. The filter is **URL-synced**
      (`/board/:id?label=…`) — sharing the link reproduces the view and refresh keeps it.

### Card detail as a modal route

- [ ] Clicking a card opens a **modal** at `/board/:id/card/:cardId` (shareable URL); a **refresh or direct
      visit shows the full card page** (React Router background-location or an overlay layout).
- [ ] Edit title, description, labels, due date; save through the API layer. Delete confirms first.
- [ ] Every mutation goes through the API layer optimistically — not just drag. Card edits, column
      renames, and board CRUD all reconcile on success and roll back on failure through one shared path.

### Drag-and-drop with optimistic persistence

- [ ] Reorder within a column and move across columns updates the UI **optimistically**, then persists via
      the API layer; on failure it **rolls back**.
- [ ] A failed save never corrupts order (no lost/duplicated cards).

### App concerns

- [ ] Route-level loading/error states + a not-found route; an activity feed on `/board/:id/activity`.
- [ ] The mock API can be made to fail on demand (a query flag, a dev-only toggle, or a seeded error rate)
      so rollback is demonstrable without editing code.
- [ ] Tests: unit tests for the reorder/move logic + component tests covering optimistic success and
      rollback. Cover every module under the API layer.

## Deploy

- [ ] Deployed to Vercel or Netlify; the live link is in the repo README.
- [ ] The production build runs clean — no type errors, no console warnings on the deployed app.

## Done check

Drag persists optimistically and rolls back on a forced failure; the label filter survives refresh and the
back button; the card modal is shareable and a refresh shows the full page; boards are per-user and the API
refuses someone else's board id; tests pass.

**Stretch (optional):** card comments/activity; assignees.
