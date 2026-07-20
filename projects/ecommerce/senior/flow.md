# Flow — E-commerce Storefront · Senior

Screen / user flow for the build.

```mermaid
flowchart TD
  Home["Catalog — streamed with Suspense + skeletons"] --> Search{"search typed fast"}
  Search -- stale response --> Drop["cancelled · never rendered"]
  Search -- newest --> Home
  Home --> PP["Product (modal / full)"]
  PP --> AddSA["Add to cart (onSubmit → route handler, optimistic)"]
  AddSA --> Stock{"last-unit race?"}
  Stock -- conflict --> Res["reconcile / rollback"]
  Stock -- ok --> Cart["Cart"]
  Home --> Login["/login (middleware)"] --> Merge["guest cart merges in"] --> Role{"Role?"}
  Role -- customer --> Shop["Storefront + checkout"]
  Cart --> Shop
  Shop --> Hold["reserve stock with an expiry"]
  Hold --> Place{"place order"}
  Place -- double submit / retry --> Idem["same idempotency key → one order"]
  Place -- ok --> Done["Confirmation"]
  Hold -- abandoned --> Rel["hold expires · stock released"]
  Role -- admin --> Admin["/admin/products"]
  Admin --> Mut["create / edit / bulk (onSubmit → route handler)"]
  Mut --> Authz{"authorized server-side?"}
  Authz -- no --> F403["refused (even if forged) · logged"]
  Authz -- ok --> Rv["revalidate caches → storefront updates · audited"]
  Rv --> Partial{"bulk: all ok?"}
  Partial -- partial fail --> RB["rollback failed items · report which"]
```

Roles run through middleware and are authorized server-side, and admin changes are audited in the route
handler so a forged request is recorded too. The catalog streams; concurrent last-unit adds reconcile;
checkout holds stock with an expiry; order placement is idempotent under retry; the modal traps and
restores focus.
