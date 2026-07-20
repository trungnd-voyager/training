# Flow — Admin Dashboard · Middle

Screen / user flow for the build.

```mermaid
flowchart TD
  Start(["Open a protected route"]) --> G{"Session?"}
  G -- no --> Login["/login?next=…"]
  Login -- success --> Ret["Return to intended route"]
  G -- yes --> Dash["Dashboard: cards + chart"]
  Ret --> Dash
  Dash --> Sec{"each section loads independently"}
  Sec -- loading --> Sp["own spinner"]
  Sec -- ok --> Data["rendered"]
  Dash --> P["Products / Orders / Customers — table state in URL (?page&sort&filter)"]
  P --> Fetch{"fetch via API layer"}
  Fetch -- loading --> Sk["skeleton"]
  Fetch -- error --> Er["error state"]
  Fetch -- ok --> L["rows"]
  L --> Edit["Create / Edit (products, customers)"] --> Opt["optimistic update"]
  Opt --> Save{"API save"}
  Save -- ok --> L
  Save -- fail --> RB["rollback → error toast"] --> L
  Dash --> Out["Logout"] --> Login
```

Table filter/sort/page live in the URL (shareable, refresh-safe, back button works). CRUD goes through the
API layer; updates apply optimistically and roll back on failure — triggerable via the failure-injection
toggle.
