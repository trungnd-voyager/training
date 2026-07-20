# Flow — Admin Dashboard · Senior

Screen / user flow for the build.

```mermaid
flowchart TD
  Login["/login"] --> Role{"Role?"}
  Role -- viewer --> RO["Read-only UI · restricted columns stripped server-side"]
  Role -- editor / admin --> RW["Full UI"]
  RW --> Table["Products: 10k virtualized + advanced filters (?stock=ge:5&status=…)"]
  Table --> Race{"filter typed fast"}
  Race -- stale response --> Drop["cancelled · never rendered"]
  Race -- newest --> Table
  Table --> Mut["Mutate / bulk select"]
  RO --> Try["Attempt mutation (forced)"]
  Mut --> Auth{"API authorizes role?"}
  Try --> Auth
  Auth -- forbidden --> F403["403 · logged"]
  Auth -- ok --> Net{"online?"}
  Net -- no --> Q["queue as pending → replay in order on reconnect"] --> Ver
  Net -- yes --> Ver{"version current?"}
  Ver -- stale --> C409["409 → resolve, don't clobber"]
  Ver -- yes --> Opt["optimistic (bulk: per item)"]
  Opt --> Res{"result"}
  Res -- all ok --> Done["applied · audited"]
  Res -- transient 5xx --> Retry["backoff retry · no double-apply"] --> Res
  Res -- partial fail --> RB["rollback failed items · report which"]
  Done --> Undo{"undo window?"}
  Undo -- used --> Inv["inverse op via API · audited"]
  Done --> Sync["other tabs: BroadcastChannel / poll → reflect"]
  Done --> Inval["cache invalidated → dashboard reflects"]
```

Authorization and auditing live in the API layer — a forced `viewer` request still gets a `403` and is
still logged, and a save against a stale version gets a `409`. Bulk actions apply optimistically and roll
back per-item on partial failure. Offline mutations queue and replay in order; every applied mutation
invalidates the cache so other views and tabs reflect it.
