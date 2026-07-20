# Project — E-commerce Storefront

Build an online storefront on **Next.js (App Router)** — server data, an intercepting
modal, route handlers, and auth. Mock data, no real payments. This is your project for the month.

## Setup

- **Stack:** Next.js (App Router) + **TypeScript**, server-first.
- **Data:** no real backend. A mock DB module (in-memory array or JSON) exposed through **route handlers**
  under `app/api/…`. **~40 products** ([**`@faker-js/faker`**](https://www.npmjs.com/package/@faker-js/faker)
  recommended) — `id, slug, name, priceCents, category, description, images[], stock, rating`. Mock users
  for login.
- **Testing:** Vitest + React Testing Library.
- **Responsive:** every screen works from mobile to desktop.

## Screens & routes

| Route                                          | Rendering        | Purpose                               |
| ---------------------------------------------- | ---------------- | ------------------------------------- |
| `/`                                            | Server Component | Catalog; server search/filter/sort    |
| `/product/[slug]`                              | Server Component | Full page; intercepted modal from `/` |
| `/category/[slug]`                             | Server Component | Category listing                      |
| `/cart`                                        | —                | Cart review                           |
| `/checkout`                                    | Protected        | Shipping form; requires login         |
| `/checkout/success`                            | —                | Confirmation                          |
| `/login`                                       | —                | Email/password login                  |
| `/admin/products`, `/admin/products/[id]/edit` | Protected        | Manage products                       |

**Shared header:** shop logo (→ `/`) + a live cart-count badge.

## Feature specs

### Catalog + search (`/`)

- [ ] Grid rendered by a **Server Component** fetching from the route handler / data module.
- [ ] **Search, filter, sort happen on the server** and are **URL-synced** (`/?q=&category=&sort=`); a shared
      link reproduces the results, the back button works; the search input is **debounced** (~300ms, replace).
- [ ] Result count + empty state.

### Quick-view modal (intercepting route)

- [ ] Clicking a product opens it in a **modal over the grid**; the URL becomes `/product/[slug]` and is
      shareable. **Refresh or direct visit shows the full product page**, not the modal.
- [ ] Implemented with a `@modal` parallel slot + `default.tsx` + a `(..)` intercepting route; Esc / backdrop
      / back closes it.

### Product page

- [ ] Server-rendered: images, name, price, description, rating, stock, quantity + add to cart;
      `notFound()` for a bad slug.

### Cart (Context + route handlers)

- [ ] Cart in a Context; "Add to cart" is an **`onSubmit`/`onClick` handler** that POSTs to a route handler,
      with an **optimistic** badge bump that reconciles on the response.
- [ ] Cart **persists in a cookie/session** (survives reload without localStorage); cart page: line items,
      quantity edit, subtotal + shipping + total, empty state.

### Auth, checkout & admin

- [ ] `/login` with email/password against mock users; sets a session. `/checkout` and `/admin/products` are
      **protected** — a logged-out visitor is redirected to `/login?next=…` and returns after logging in.
- [ ] Logout clears the session; a stale session is rejected by the route handler, not just the UI.
- [ ] A **guest cart survives logging in** — items added before signing in merge into the user's cart
      rather than being dropped or duplicated.
- [ ] Checkout: validated form submitted through `onSubmit` + summary → `/checkout/success`, cart cleared;
      empty-cart direct visit → `/`.
- [ ] Checkout **re-checks stock server-side** before confirming; a line that went out of stock while in
      the cart blocks the order with a clear message instead of silently succeeding.
- [ ] Admin products list + create/edit through route handlers; an admin edit invalidates the catalog
      cache so the storefront reflects it.

### Framework concerns

- [ ] `loading.tsx`, `error.tsx`, `not-found.tsx` on the relevant segments.
- [ ] **Caching:** catalog cached/revalidated; cart and user data use `no-store`. Prices from integer cents.
- [ ] `'use client'` appears **only** on interactive leaves, not whole pages.
- [ ] The route handlers can be made to fail on demand (a query flag, a dev-only toggle, or a seeded error
      rate) so optimistic rollback is demonstrable without editing code.
- [ ] Rapid repeated "add to cart" clicks settle on the correct quantity — a slow earlier response never
      overwrites a newer one.
- [ ] Tests: unit tests for the cart logic + component tests covering optimistic success and rollback.

## Deploy

- [ ] Deployed to Vercel; the live link is in the repo README.
- [ ] `next build` completes with no type errors, and the deployed app has no console warnings.

## Done check

Click-to-modal and direct-URL-to-full-page both work; search survives refresh and the back button;
logged-out checkout redirects and returns and a guest cart survives the login; the cart is correct after
rapid optimistic adds and rolls back on a forced failure; an out-of-stock line blocks the order; tests pass.

**Stretch (optional):** pagination or infinite scroll; an order-history page for the logged-in user.
