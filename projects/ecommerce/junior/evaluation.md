# Evaluation — E-commerce Storefront · Junior

## Specs

- [ ] Catalog: server-rendered grid; category filter derived from data; client search; sort; count; empty state
- [ ] Product page (images, price, stock, add to cart); "not found" on a bad slug
- [ ] Quick-view modal from the catalog
- [ ] Category page
- [ ] Cart (client Context, localStorage): line items, qty edit, totals, empty state; header count live
- [ ] Shipping is a stated rule, not a magic number; quantity clamped to stock; re-adding increments
- [ ] Checkout: validated form → success (order number); empty-cart visit → `/`
- [ ] Placing an order decrements stock; a product can sell out
- [ ] Admin products CRUD behind a mock login; logout re-gates the admin
- [ ] An admin edit shows on the public catalog, not just the admin table
- [ ] Prices via `Intl`; loading skeletons; responsive; no console warnings
- [ ] Tests for cart maths + catalog filter/search/sort, and they pass
- [ ] Deployed; link works

## Code quality

- [ ] Strict TypeScript
- [ ] `'use client'` only on interactive leaves — pages stay Server Components
- [ ] Cart totals correct; state immutable
- [ ] Can explain why a browser-only change is invisible to a Server Component
- [ ] Tests assert behaviour, not implementation detail

## Workflow

- [ ] Talked through the flow before diving in
- [ ] Can explain the server/client split they chose
- [ ] Can explain any line
- [ ] Bonus: did a Stretch item

## Verdict

- [ ] Right at Junior
- [ ] Ahead
- [ ] Not there yet
