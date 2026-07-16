# Flow — Admin Dashboard · Senior

Screen / user flow for the build.

```mermaid
flowchart TD
  Login["/login"] --> Role{"Role?"}
  Role -- viewer --> RO["Read-only UI (mutations hidden)"]
  Role -- editor / admin --> RW["Full UI"]
  RW --> Table["Products: 10k virtualized + advanced filters (?stock=ge:5&status=…)"]
  Table --> Mut["Mutate / bulk select"]
  RO --> Try["Attempt mutation (forced)"]
  Mut --> Auth{"API authorizes role?"}
  Try --> Auth
  Auth -- forbidden --> F403["403"]
  Auth -- ok --> Opt["optimistic (bulk: per item)"]
  Opt --> Res{"result"}
  Res -- all ok --> Done["applied · activity logged"]
  Res -- partial fail --> RB["rollback failed items"]
  Done --> Sync["other tabs poll → reflect"]
```

Authorization lives in the API layer — a forced `viewer` request still gets a `403`. Bulk actions apply
optimistically and roll back per-item on partial failure; other tabs pick up changes by polling.
