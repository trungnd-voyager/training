# Project — Admin Dashboard

Build a production-grade internal back-office to manage a shop — products, orders,
and customers — with role-based access, auditability, and large-dataset performance. Data comes from a
mock API layer, no real backend. This is your project for the month.

## Setup

- **Stack:** React + Vite + **TypeScript**, with **React Router**.
- **Data:** a **mock API layer** over an in-memory + **localStorage** dataset (async, latency, query params).
  **[MSW](https://mswjs.io/)** recommended. Seed a resource with **~10,000 rows** to exercise performance
  ([**`@faker-js/faker`**](https://www.npmjs.com/package/@faker-js/faker) recommended): products carry
  `priceCents`, `stock`, `category`, `active`; orders a `status`; customers an `orderCount`. Mock users
  carry a `role` of `admin` / `editor` / `viewer`.
- **Testing:** Vitest + React Testing Library + **Playwright** (E2E).
- **Responsive:** shell and every screen work down to mobile.

## Screens & routes

| Route                                                  | Screen       | Purpose                             |
| ------------------------------------------------------ | ------------ | ----------------------------------- |
| `/login`                                               | Login        | Session sign-in                     |
| `/`                                                    | Dashboard    | Metrics + chart; reflects mutations |
| `/products`                                            | Products     | Advanced filters; virtualized       |
| `/products/new`, `/products/:id`, `/products/:id/edit` | Product CRUD | Authorized by role in the API layer |
| `/orders`                                              | Orders       | Advanced filters                    |
| `/customers`                                           | Customers    | Advanced filters                    |
| `/activity`                                            | Activity log | Audit feed (who / what / when)      |

**Shared shell:** sidebar (Dashboard, Products, Orders, Customers, Activity) + top bar; nav adapts to the
signed-in user's role; collapses on mobile.

## Feature specs

### Core

- [ ] Tables via the mock API layer with pagination/sort/filter as URL-synced query params;
      shareable, refresh-safe.
- [ ] CRUD via the API layer with optimistic updates; dashboard sections load independently and reflect
      a mutation without a full reload.
- [ ] **Request cancellation:** typing in a filter fires overlapping requests; a slow earlier response
      must never overwrite a newer one. Demonstrate with staggered latency in the mock API.
- [ ] A **query cache** — in-flight dedup, stale-while-revalidate, and explicit invalidation on mutation.
      Use TanStack Query or write your own; either way be able to explain the invalidation boundaries and
      why a mutation refreshes exactly the views it should.

### Role-based access — enforced in the API layer

- [ ] Roles `admin` / `editor` / `viewer`. The API layer authorizes every mutation and rejects an
      unauthorized one even if the UI allowed it — the check lives in the data layer, not the component.
      The UI also adapts to role.
- [ ] **Column-level permissions:** at least one field (e.g. `priceCents`) is invisible to `viewer` and
      read-only for `editor`. The API layer strips it from responses and rejects writes to it — hiding
      the input is not sufficient.

### Advanced querying

- [ ] API query operators: numeric ranges (e.g. `stock=ge:5`), multi-value OR on status, composable with
      sort and pagination.
- [ ] Filter state is parsed and validated at the boundary — a hand-edited URL with a garbage operator
      produces a clear error, not a crash or a silently ignored filter.

### Performance at scale

- [ ] The 10k-row resource scrolls as one continuous list: the API serves it in pages, the client
      accumulates them as the user scrolls, and **virtualization/windowing**
      (e.g. `@tanstack/react-virtual` or `react-window`) keeps only the visible window in the DOM.
      Filtering or sorting resets the accumulation and re-queries the API — it does not sort in memory.
- [ ] The virtualized table keeps a **sticky header**, supports **variable row heights**, and stays fully
      keyboard-navigable — arrow keys move the focused row and scroll it into view without losing focus
      as rows unmount.
- [ ] Measure it: record scroll FPS / interaction latency before and after virtualization, and put the
      two numbers plus your method in the repo README.
- [ ] Justify one memoization decision with a profile — show the render you eliminated.

### Bulk, freshness & audit

- [ ] **Bulk actions** (multi-select, including select-all-matching-filter across pages) with optimistic UI
      and **rollback on partial failure** — the result reports which items failed and why.
- [ ] **Undo** on destructive and bulk actions within a short window, applied as a real inverse operation
      through the API layer and itself recorded in the audit log.
- [ ] Freshness across tabs: changes made in one tab appear in another. Polling is the floor —
      `BroadcastChannel` or the `storage` event is better. Whichever you pick, justify the trade-off.
- [ ] **Concurrent edits:** records carry a version. Saving against a stale version is rejected by the API
      layer and the UI offers a resolution — it never silently overwrites the other write.
- [ ] An **activity log** — append-only, written **inside the API layer** so a forced or scripted request
      is logged too, recording actor / action / entity / before → after / timestamp. Filterable by actor
      and entity on `/activity`.
- [ ] Distinguish transient from permanent failures: retry 5xx with backoff, surface 4xx immediately.
      A retried mutation must not apply twice.
- [ ] **Offline queue:** mutations made while offline are queued, surfaced as pending in the UI, and
      replayed in order on reconnect — with conflicts and rejections resolved, not dropped.

### Accessibility

- [ ] Keyboard-navigable tables and menus; correct ARIA roles; focus management and focus-trap in dialogs,
      with focus restored to the trigger on close.
- [ ] Live regions announce async outcomes (save, bulk result, validation errors) to a screen reader.
- [ ] **Zero axe violations** on every route, asserted in a test — not a manual pass.

### Testing

- [ ] Meaningful unit + integration tests, including the query/cache layer and the authorization rules.
- [ ] **Playwright E2E covering:** login → edit → reflected in the table; a `viewer`'s forced mutation
      getting `403`; a bulk action with a seeded partial failure; a stale-version `409` and its resolution;
      a keyboard-only path through the table and a dialog.

## Deploy

- [ ] Deployed to Vercel or Netlify; the live link is in the repo README.
- [ ] The production build runs clean — no type errors, no console warnings on the deployed app.

## Done check

The E2E suite passes; a `viewer`'s mutation is rejected by the API layer even when forced through, and
a restricted column never reaches the client; a fast-typed filter never renders a stale response; the
10k-row table filters and scrolls without jank, keyboard-navigates, and the before/after numbers are
written down; a stale-version save is refused rather than clobbering; a bulk action reports exactly which
items failed; undo restores through the API and is itself audited; mutations made offline replay in order
on reconnect; axe is clean on every route.

**Stretch (optional):** multi-tenant workspaces; a saved-views feature with shareable permalinks.
