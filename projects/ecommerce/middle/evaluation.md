# Evaluation — E-commerce Storefront · Middle

## Specs

- [ ] Catalog Server Component; server search/filter/sort URL-synced, debounced; count + empty state
- [ ] Quick-view via an intercepting route; refresh/direct → full page; shareable URL
- [ ] Product page; `notFound()` on a bad slug
- [ ] Cart Context + route handlers via `onSubmit`, optimistic; persists in a cookie/session
- [ ] Rapid repeated adds settle on the correct quantity — no out-of-order overwrite
- [ ] Login session; `/checkout` + `/admin` protected (redirect + return); logout clears it
- [ ] Guest cart merges into the user's cart on login — not dropped, not duplicated
- [ ] Checkout re-checks stock server-side; an out-of-stock line blocks the order
- [ ] Admin edit invalidates the catalog cache; the storefront reflects it
- [ ] Failure injection exists; rollback demonstrable without a code edit
- [ ] loading / error / not-found; caching (catalog revalidated, cart `no-store`)
- [ ] Tests for the cart logic + optimistic success and rollback
- [ ] Deployed; link works

## Code quality

- [ ] Strict TypeScript
- [ ] `'use client'` only on leaves, not whole pages
- [ ] Optimistic cart without corruption
- [ ] Cart merge handles the same product in both carts without double-counting

## Workflow

- [ ] Talked through the flow before diving in
- [ ] Can explain the intercepting-route modal
- [ ] Can explain any line
- [ ] Bonus: did a Stretch item

## Verdict

- [ ] Right at Middle
- [ ] Ahead
- [ ] Not there yet
