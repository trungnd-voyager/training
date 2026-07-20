# Evaluation — E-commerce Storefront · Senior

## Specs

- [ ] Core holds: server search URL-synced, intercepting modal, route-handler cart, protected checkout
- [ ] Catalog streams with Suspense + skeletons; parallel fetches; long lists paginated/virtualized
- [ ] Stale responses cancelled — fast typing never renders an out-of-order result
- [ ] Guest cart merges into the user's cart on login; same product combines once, by a stated rule
- [ ] Order placement idempotent — double-submit, retry, or refresh creates exactly one order
- [ ] Middleware auth; roles customer/admin; admin route handlers authorize server-side (a forged request is refused)
- [ ] Editing a product revalidates the right caches; can say what was deliberately left alone
- [ ] Bulk admin actions, rollback on partial failure, naming which products failed
- [ ] Admin audit log written inside the route handler — a forged request is recorded too
- [ ] Stock / concurrent last-unit handling; reservation expires and releases on an abandoned checkout
- [ ] Retry with backoff on 5xx; no double-apply; 4xx surfaced immediately
- [ ] Optimistic reconcile + rollback; error boundaries per route
- [ ] Focus trap + restore in the modal; keyboard throughout
- [ ] Zero axe violations, asserted in a test; live regions announce async outcomes
- [ ] Playwright covers the full matrix (checkout, forged admin request, double-submit, cart merge, keyboard)
- [ ] Deployed; link works

## Code quality

- [ ] Strict TypeScript
- [ ] Authorization **and** auditing server-side, not just in the UI
- [ ] LCP/CLS before and after written down, with the method
- [ ] Memoization justified by a profile
- [ ] Real unit + integration tests

## Workflow

- [ ] Talked through the flow before diving in
- [ ] Weighs trade-offs (caching/revalidation, concurrency, a11y) on their own
- [ ] Can explain any line
- [ ] Bonus: did a Stretch item

## Verdict

- [ ] Right at Senior
- [ ] Below Senior
