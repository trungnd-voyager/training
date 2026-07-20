# Flow — Kanban Board · Middle

Screen / user flow for the build.

```mermaid
flowchart TD
  Start(["Open protected route"]) --> G{"Session?"}
  G -- no --> Login["/login"]
  Login -- success --> Boards["Boards list"]
  G -- yes --> Boards
  Boards --> Board["Board — loads from the API layer"]
  Board --> DnD["Any mutation: drag, card edit, column rename, board CRUD"] --> Opt["optimistic"]
  Opt --> Save{"persist via API"}
  Save -- ok --> Board
  Save -- fail --> RB["rollback (no corruption)"] --> Board
  Board --> Filter["Label filter → ?label= in the URL"] --> Board
  Board --> Click["Open a card"]
  Click --> How{"how was it opened?"}
  How -- clicked from the board --> Modal["Modal route (shareable URL)"]
  How -- refresh / direct --> Full["Full card page"]
  Boards --> Out["Logout"] --> Login
```

Card detail is a modal route with a full-page fallback. Every mutation persists through the API layer
optimistically and rolls back on failure — triggerable via the failure-injection toggle. The label filter
lives in the URL.
