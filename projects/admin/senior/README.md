# Project — Admin Dashboard

Build a production-grade internal back-office to manage a shop — products, orders,
and customers — with role-based access, auditability, and large-dataset performance. Data comes from a
mock API layer, no real backend. This is your project for the month.

## Setup

- **Stack:** React + Vite + **TypeScript**, with **React Router**. Avoid `any`.
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

### Role-based access — enforced in the API layer

- [ ] Roles `admin` / `editor` / `viewer`. The API layer authorizes every mutation and rejects an
      unauthorized one even if the UI allowed it — the check lives in the data layer, not the componen. The UI also adapts to role.

### Advanced querying

- [ ] API query operators: numeric ranges (e.g. `stock=ge:5`), multi-value OR on status, composable with
      sort and pagination.

### Performance at scale

- [ ] The 10k-row resource uses **virtualization/windowing** (e.g. `@tanstack/react-virtual` or
      `react-window`) with API pagination, and stays smooth (no jank, no rendering 10k DOM nodes).

### Bulk, freshness & audit

- [ ] **Bulk actions** (multi-select) with optimistic UI and **rollback on partial failure**.
- [ ] Near-real-time updates via polling the API layer.
- [ ] An **activity log** records who changed what and when, surfaced on `/activity`.

### Accessibility

- [ ] Keyboard-navigable tables and menus; correct ARIA roles; focus management in dialogs.

### Testing

- [ ] Meaningful unit + integration tests.
- [ ] **At least one Playwright E2E:** login → edit a record → see it reflected in the table.

## Done check

The E2E passes; a `viewer`'s mutation is rejected by the API layer even when forced through; the 10k-row
table filters and scrolls without jank; the activity log reflects real changes; the dashboard reflects a
mutation without a full reload.

**Stretch (optional):** multi-tenant workspaces; column-level permissions per role.
