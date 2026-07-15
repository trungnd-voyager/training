# Project — E-commerce Storefront

Build a production-grade online storefront on **Next.js (App Router)** — server data,
auth with roles, performance, resilience, and a real test suite. Mock data, no real payments. This is your
project for the month.

## Setup

- **Stack:** Next.js (App Router) + **TypeScript**, server-first. Avoid `any`.
- **Data:** a mock DB module exposed through **route handlers** under `app/api/…`. **~40 products**
  ([**`@faker-js/faker`**](https://www.npmjs.com/package/@faker-js/faker) recommended) —
  `id, slug, name, priceCents, category, description, images[], stock, rating`. Mock users with a `role`
  of `customer` or `admin`.
- **Testing:** Vitest + React Testing Library + **Playwright** (E2E).
- **Responsive:** every screen works from mobile to desktop.

## Screens & routes

| Route                                          | Notes                                                                                   |
| ---------------------------------------------- | --------------------------------------------------------------------------------------- |
| `/`                                            | Server-rendered catalog; server search/filter/sort, URL-synced; streams with `Suspense` |
| `/product/[slug]`                              | Full product page; intercepted as a **quick-view modal** from `/`                       |
| `/category/[slug]`                             | Category listing                                                                        |
| `/cart`, `/checkout`, `/checkout/success`      | Cart (cookie/session) → protected checkout → confirmation                               |
| `/login`                                       | Session login                                                                           |
| `/admin/products`, `/admin/products/[id]/edit` | **Admin-only** product management                                                       |

**Shared header:** shop logo (→ `/`) + a live cart-count badge.

## Feature specs

### Core storefront

- [ ] Server-rendered catalog with **server-side** search/filter/sort, URL-synced (`/?q=&category=&sort=`),
      debounced input.
- [ ] **Quick-view modal** via an intercepting route (`@modal` slot + `default.tsx` + `(..)` matcher): click →
      modal; refresh/direct URL → full page; shareable URL.
- [ ] Cart via Context + **Server Actions**, persisted in a cookie/session; `/checkout` protected with
      redirect + return; `loading` / `error` / `not-found` on the relevant segments.

### Performance

- [ ] Stream the catalog with **`Suspense`** and per-section skeletons; the page shell paints before data.
- [ ] Independent data is fetched in **parallel**, not a waterfall.
- [ ] Long lists use **server pagination or virtualization**; images are optimized.

### Auth, roles, admin

- [ ] Sessions + **middleware** route protection. Roles `customer` / `admin`; an `/admin/products` area
      (list + create/edit) gated to `admin`.
- [ ] The admin **Server Actions authorize server-side** — a non-admin request is refused even if the UI is
      bypassed or the request is forged.
- [ ] Editing a product **invalidates the right caches** (`revalidatePath`/tags) so the storefront reflects
      the change.

### Correctness & resilience

- [ ] Stock checks and out-of-stock handling; a defined resolution for two concurrent adds of the last unit.
- [ ] Optimistic UI **reconciles on success and rolls back on error**.
- [ ] Error boundaries per route; the app degrades gracefully when a data call fails (no white screen).

### Accessibility

- [ ] Keyboard navigation throughout; correct semantics; **focus trap + restore** in the modal.

### Testing

- [ ] Meaningful unit + integration tests.
- [ ] **At least one Playwright E2E:** browse → add to cart → checkout → confirmation.

## Done check

The E2E passes; an admin edit shows up for customers; a non-admin is refused **server-side** on admin
routes/actions; the app stays usable when a data call fails; optimistic actions roll back on error; no
layout shift or hydration warnings.

**Stretch (optional):** coupon codes; multi-currency; a Stripe _test-mode_ checkout stub.
