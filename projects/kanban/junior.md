# Project — Kanban Board

Build a task board — boards of draggable cards, like a simple Trello.
Mock data, no real backend. This is your project for the month.

## Setup

- **Stack:** React + Vite + **TypeScript**, with **React Router**. Avoid `any`.
- **Drag-and-drop:** use a library — **dnd-kit** is recommended (it's accessible). Don't hand-roll HTML5
  drag events.
- **Data:** mock data in a module, stored **normalized** (this shape matters — it's part of the exercise),
  **persisted to localStorage** (survives reload)
  ([**`@faker-js/faker`**](https://www.npmjs.com/package/@faker-js/faker) recommended for seed cards). The UI
  reads and writes the store directly:

  ```ts
  interface Card {
    id: string;
    title: string;
    description: string;
    labels: string[];
    dueDate?: string;
  }
  interface Column {
    id: string;
    title: string;
    cardIds: string[];
  }
  interface Board {
    id: string;
    ownerId: string;
    title: string;
    columnOrder: string[];
  }
  ```

- **Responsive:** the board works down to mobile — columns scroll horizontally.

## Screens & routes

| Route                     | Screen       | Purpose                                                    |
| ------------------------- | ------------ | ---------------------------------------------------------- |
| `/login`                  | Login        | Mock sign-in                                               |
| `/`                       | Boards list  | The user's boards; create / rename / delete                |
| `/board/:id`              | Board view   | Columns + cards; drag to move/reorder                      |
| `/board/:id/card/:cardId` | Card detail  | Modal over the board; a direct visit / refresh = full page |
| `/board/:id/activity`     | Activity log | Feed of moves & edits                                      |

## Feature specs

### Auth & boards

- [ ] A **mock login** against hardcoded users; a route guard sends logged-out visitors to `/login`
      (a localStorage session is enough).
- [ ] `/` lists the user's boards (a board belongs to its owner); create / rename / delete a board.

### Board view

- [ ] Columns left-to-right; each shows a card count; an empty column shows a placeholder, not a blank gap.
- [ ] A card shows title, a description indicator, label chips, and a due-date pill.
- [ ] Add / rename (inline) / delete columns (delete confirms).

### Cards

- [ ] Inline "add card" at the bottom of a column.
- [ ] Clicking a card opens the detail at `/board/:id/card/:cardId` as a **modal over the board**; a
      **direct visit / refresh shows the full card page**.
- [ ] Edit title, description, labels, due date; delete (with confirm).
- [ ] **Filter** a board's cards by label.

### Drag-and-drop

- [ ] Reorder a card **within** a column and move **across** columns, dropping at the correct position.
- [ ] Reordering updates the normalized state **immutably** — no lost or duplicated cards.

### Activity & global

- [ ] Activity log lists the moves / edits made, kept in local state.
- [ ] The board **persists across reload** (localStorage); responsive; zero console warnings.

## Done check

Cards drag within and across columns and survive reload; board/column/card CRUD all work; the modal is shareable and a
refresh shows the full page; state stays consistent (never a lost or duplicated card).

**Stretch (optional):** card colors beyond basic labels; a WIP limit per column.
