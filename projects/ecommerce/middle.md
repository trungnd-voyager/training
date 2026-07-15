# Project — E-commerce Storefront

Build an online storefront on **Next.js (App Router)** — server data, an intercepting
modal, Server Actions, and auth. Mock data, no real payments. This is your project for the month.

## Setup

- **Stack:** Next.js (App Router) + **TypeScript**, server-first. Avoid `any`.
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

### Cart (Context + Server Actions)

- [ ] Cart in a Context; "Add to cart" is a **Server Action** with an **optimistic** badge bump.
- [ ] Cart **persists in a cookie/session** (survives reload without localStorage); cart page: line items,
      quantity edit, subtotal + shipping + total, empty state.

### Auth, checkout & admin

- [ ] `/login` with email/password against mock users; sets a session. `/checkout` and `/admin/products` are
      **protected** — a logged-out visitor is redirected to `/login?next=…` and returns after logging in.
- [ ] Checkout: validated form + summary → `/checkout/success`, cart cleared; empty-cart direct visit → `/`.
- [ ] Admin products list + create/edit through route handlers.

### Framework concerns

- [ ] `loading.tsx`, `error.tsx`, `not-found.tsx` on the relevant segments.
- [ ] **Caching:** catalog cached/revalidated; cart and user data use `no-store`. Prices from integer cents.
- [ ] `'use client'` appears **only** on interactive leaves, not whole pages.
- [ ] Tests: unit tests for the cart logic + 2–3 component tests.

## Done check

Click-to-modal and direct-URL-to-full-page both work; search survives refresh and the back button;
logged-out checkout redirects and returns; the cart is correct after rapid optimistic adds; tests pass.

**Stretch (optional):** pagination or infinite scroll; an order-history page for the logged-in user.
