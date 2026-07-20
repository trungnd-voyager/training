# Flow — E-commerce Storefront · Middle

Screen / user flow for the build.

```mermaid
flowchart TD
  Home["Catalog (Server Component)"] --> S["server search / filter / sort — URL-synced, debounced"]
  S --> Cards["cards"]
  Cards --> Click["Click a product"]
  Click --> Modal["Quick-view (intercepting route, shareable)"]
  Click -- refresh / direct --> PP["Full product page (notFound on bad slug)"]
  Cards --> AddSA["Add to cart (onSubmit → route handler, optimistic)"]
  AddSA --> Save{"handler result"}
  Save -- ok --> Cart["Cart (cookie/session)"]
  Save -- fail --> RB["rollback → error"] --> Cart
  Cart --> Guard{"logged in?"}
  Guard -- no --> Login["/login?next=/checkout"] --> Merge["guest cart merges into the user's"] --> Checkout
  Guard -- yes --> Checkout["Checkout"]
  Checkout --> Stock{"stock still available?"}
  Stock -- no --> Blocked["blocked with a clear message"] --> Cart
  Stock -- yes --> Success["Success"]
```

Search is server-side and URL-synced; the quick-view is an intercepting route with a full-page fallback; the
cart posts to route handlers from `onSubmit`, applies optimistically with rollback on failure, and persists
in a cookie/session. Stock is re-checked server-side before the order confirms.
