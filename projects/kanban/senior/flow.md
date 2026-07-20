# Flow — Kanban Board · Senior

Screen / user flow for the build.

```mermaid
flowchart TD
  Login["/login"] --> Role{"Board role?"}
  Role -- member --> RO["Limited UI (no board delete)"]
  Role -- owner --> RW["Full UI"]
  RW --> Act["Move / edit a card · bulk select"]
  RO --> Try["Owner-only action (forced)"]
  Act --> Auth{"API authorizes?"}
  Try --> Auth
  Auth -- forbidden --> F403["403 · logged"]
  Auth -- ok --> Net{"online?"}
  Net -- no --> Q["queue as pending → replay in order on reconnect"] --> Ver
  Net -- yes --> Ver{"card version current?"}
  Ver -- stale --> Res["conflict resolution (no lost/dup)"]
  Ver -- yes --> Opt["optimistic (bulk: per card)"]
  Opt --> Result{"result"}
  Result -- ok --> Done["applied · audited"]
  Result -- transient 5xx --> Retry["backoff retry · no double-apply"] --> Result
  Result -- partial fail --> RB["rollback failed cards · report which"]
  Done --> Undo{"undo window?"}
  Undo -- used --> Inv["inverse op via API · audited"]
  Done --> Sync["broadcast → other tabs update"]
  Done --> Inval["cache invalidated"]
  RW --> KB["Keyboard drag + screen-reader announcements"]
```

Roles and auditing are enforced in the API — a forced `member` request still gets a `403` and is still
logged. Other tabs stay in sync (storage / BroadcastChannel / polling); a concurrent move on the same card
resolves without corruption. Offline moves queue and replay in order; every applied mutation invalidates
the cache.
