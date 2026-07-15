# Project — Admin Dashboard

Build an internal back-office to manage a shop — products, orders, and customers.
Mock data, no real backend. This is your project for the month.

## Setup

- **Stack:** React + Vite + **TypeScript**, with **React Router**.
- **Data:** no real backend. Mock data in a module **persisted to localStorage** (survives reload) — seed
  **~60 products, ~40 orders, ~30 customers**
  ([**`@faker-js/faker`**](https://www.npmjs.com/package/@faker-js/faker) recommended):

  ```ts
  interface Product {
    id: string;
    name: string;
    sku: string; // "NK-KEY-014"
    priceCents: number; // integer cents
    stock: number;
    category: string; // Keyboards | Mice | Monitors | Cables | Audio
    active: boolean;
  }
  interface Order {
    id: string;
    customerEmail: string;
    placedAt: string;
    status: "pending" | "shipped" | "delivered" | "cancelled";
    totalCents: number;
  }
  interface Customer {
    id: string;
    name: string;
    email: string;
    createdAt: string;
    orderCount: number;
  }
  ```

- **Responsive:** the shell and every screen must work down to mobile — the sidebar collapses to a toggle.

## Screens & routes

| Route                                                  | Screen       | Purpose                      |
| ------------------------------------------------------ | ------------ | ---------------------------- |
| `/login`                                               | Login        | Mock sign-in                 |
| `/`                                                    | Dashboard    | Summary tiles                |
| `/products`                                            | Products     | List, search, sort, paginate |
| `/products/new`, `/products/:id`, `/products/:id/edit` | Product CRUD | Create / view / edit         |
| `/orders`                                              | Orders       | List, search, sort, paginate |
| `/customers`                                           | Customers    | List, search, sort, paginate |
| `/activity`                                            | Activity log | Feed of changes made         |

**Shared shell** on every screen: a left sidebar (Dashboard, Products, Orders, Customers, Activity) with
the active item highlighted, and a top bar. On mobile the sidebar collapses to a toggle.

## Feature specs

### Shell & auth

- [ ] Responsive shell; the active nav item reflects the current route.
- [ ] A mock login against hardcoded users; a route guard sends logged-out visitors to `/login`

### Products

- [ ] Table columns: name, SKU, category, price, stock, status, row actions (View / Edit / Delete).
- [ ] Client-side search (name or SKU, live), sortable column headers, pagination (10/page) with a
      "showing 1–10 of 60" line; searching resets to page 1; empty state when nothing matches.
- [ ] Prices formatted from cents; low-stock rows (`stock < 5`) flagged.
- [ ] Create / Edit: a form with per-field validation (name/SKU required, price & stock ≥ 0,
      SKU unique); submit disabled until valid; on success a toast + the change visible.
- [ ] Detail: read-only view with Edit / Delete; a bad id shows "Product not found", not a crash.
- [ ] Delete confirms first, then removes and toasts.

### Orders & Customers

- [ ] Read-only tables with client-side search / sort / pagination, formatted currency and dates, empty states.

### Dashboard & Activity

- [ ] Dashboard summary tiles (total products, out-of-stock count, orders, customers).
- [ ] Activity log lists the create / edit / delete actions performed, kept in local state.

### Global

- [ ] Success/error toasts; loading & empty states with simulated latency.
- [ ] Data persists across reload (localStorage); zero console warnings.

## Done check

Every screen works client-side and is responsive; create/edit/delete update state immutably and survive
reload; search + sort + pagination interact correctly; the form can't submit invalid data.

**Stretch (optional):** a column show/hide toggle; CSV export of a table.
