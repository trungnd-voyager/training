# Project — Admin Dashboard

Build an internal back-office to manage a shop — products, orders, and customers.
Data comes from a mock API layer, not a real backend. This is your project for the month.

## Setup

- **Stack:** React + Vite + **TypeScript**, with **React Router**.
- **Data:** no real backend. A **mock API layer** over an in-memory + **localStorage** dataset (async, latency, query params).
  **[MSW (Mock Service Worker)](https://mswjs.io/)** recommended. Seed **~60 products, ~40 orders,
  ~30 customers** ([**`@faker-js/faker`**](https://www.npmjs.com/package/@faker-js/faker) recommended):
  products carry `priceCents`, `stock`, `category`, `active`; orders a `status`; customers an `orderCount`.
  Plus mock users for login.
- **Testing:** Vitest + React Testing Library.
- **Responsive:** shell and every screen work down to mobile.

## Screens & routes

| Route                                                  | Screen       | Purpose                   |
| ------------------------------------------------------ | ------------ | ------------------------- |
| `/login`                                               | Login        | Session sign-in           |
| `/`                                                    | Dashboard    | Metric cards + a chart    |
| `/products`                                            | Products     | Query via API, URL-synced |
| `/products/new`, `/products/:id`, `/products/:id/edit` | Product CRUD | Create / view / edit      |
| `/orders`                                              | Orders       | Query via API, URL-synced |
| `/customers`                                           | Customers    | Query via API, URL-synced |
| `/activity`                                            | Activity log | Feed of changes           |

**Shared shell:** sidebar (Dashboard, Products, Orders, Customers, Activity) + top bar; active item marked;
collapses on mobile.

## Feature specs

### Auth

- [ ] `/login` against mock users; store a session (memory + localStorage).
- [ ] The whole admin is protected by a **route guard** — logged-out visitors are redirected to `/login`.

### Data tables (Products, Orders, Customers)

- [ ] Each table pulls from the mock API layer.
- [ ] Pagination, sort, and filtering are handled as API query params and synced to the URL
      (`useSearchParams`), e.g. `/products?page=2&sort=price:desc&category=Keyboards`. Sharing the URL
      reproduces the view; refresh keeps it.
- [ ] Columns, cents → currency formatting, and empty states per resource.

### CRUD (Products)

- [ ] Create / edit / delete through the API layer, with optimistic updates that roll back on failure.
- [ ] Controlled form + validation (name/SKU required, price/stock ≥ 0, SKU unique); delete confirms first.

### Dashboard & Activity

- [ ] Metric cards (total products, orders today, revenue) + one **chart** from mock aggregates.
- [ ] Each dashboard section fetches its own data and loads independently — a slow section shows its
      own spinner and doesn't block the rest of the page.
- [ ] Activity feed on `/activity` records create / edit / delete through the API layer.

### App concerns

- [ ] A consistent loading pattern per route; an error boundary; a catch-allnot-found route.
- [ ] Tests: unit tests for the query/table logic (sort/filter/paginate) + a couple of component tests.

## Done check

Table state lives in the URL and survives refresh + back; a slow dashboard section doesn't block the table;
optimistic edits reconcile and roll back on a forced failure; logged-out access redirects; tests pass.

**Stretch (optional):** CSV export; saved/named filters.
