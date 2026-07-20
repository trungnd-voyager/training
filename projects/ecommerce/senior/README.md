# Project — E-commerce Storefront

Build a production-grade online storefront on **Next.js (App Router)** — server data,
auth with roles, performance, resilience, and a real test suite. Mock data, no real payments. This is your
project for the month.

## Setup

- **Stack:** Next.js (App Router) + **TypeScript**, server-first.
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
- [ ] Cart via Context + **route handlers** (mutations fired from `onSubmit`/`onClick`, not form actions),
      persisted in a cookie/session; `/checkout` protected with redirect + return; `loading` / `error` /
      `not-found` on the relevant segments.
- [ ] A **guest cart merges into the user's cart on login** — same product in both carts combines once,
      with a stated rule for whose quantity wins.
- [ ] **Order placement is idempotent:** a double-submit, a retry, or a refresh of the confirmation never
      creates a second order. Say what key you used and why.

### Performance

- [ ] Stream the catalog with **`Suspense`** and per-section skeletons; the page shell paints before data.
- [ ] Independent data is fetched in **parallel**, not a waterfall.
- [ ] Long lists use **server pagination or virtualization**; images are optimized.
- [ ] Measure it: record LCP and CLS for the catalog before and after your streaming/image work, and put
      the numbers plus your method in the repo README. Justify one memoization decision with a profile.

### Auth, roles, admin

- [ ] Sessions + **middleware** route protection. Roles `customer` / `admin`; an `/admin/products` area
      (list + create/edit) gated to `admin`.
- [ ] The admin **route handlers authorize server-side** — a non-admin request is refused even if the UI is
      bypassed or the request is forged (e.g. a raw `curl` POST).
- [ ] Editing a product **invalidates the right caches** (`revalidatePath`/tags) so the storefront reflects
      the change — and you can say which entries you deliberately left alone.
- [ ] **Bulk admin actions** (multi-select price change or deactivate) with **rollback on partial failure**;
      the result names which products failed and why.
- [ ] An **audit log** of admin changes — actor / product / before → after / timestamp — written **inside
      the route handler**, so a forged or scripted request is recorded too.

### Correctness & resilience

- [ ] Stock checks and out-of-stock handling; a defined resolution for two concurrent adds of the last unit.
- [ ] Stock is **reserved at checkout with an expiry**, not merely read — an abandoned checkout releases
      its hold instead of stranding inventory forever.
- [ ] Optimistic UI **reconciles on success and rolls back on error**.
- [ ] **Request cancellation:** typing in search fires overlapping requests; a slow earlier response must
      never overwrite a newer one. Demonstrate with staggered latency.
- [ ] Distinguish transient from permanent failures: retry 5xx with backoff, surface 4xx immediately.
      A retried mutation must not apply twice.
- [ ] Error boundaries per route; the app degrades gracefully when a data call fails (no white screen).

### Accessibility

- [ ] Keyboard navigation throughout; correct semantics; **focus trap + restore** in the modal.
- [ ] Live regions announce async outcomes (added to cart, stock rejection, bulk result) to a screen reader.
- [ ] **Zero axe violations** on every route, asserted in a test — not a manual pass.

### Testing

- [ ] Meaningful unit + integration tests, including the authorization rules and the cart-merge logic.
- [ ] **Playwright E2E covering:** browse → add to cart → checkout → confirmation; a non-admin's forged
      admin request refused server-side; a double-submitted order creating only one order; a guest cart
      surviving login; a keyboard-only path through the quick-view modal.

## Deploy

- [ ] Deployed to Vercel; the live link is in the repo README.
- [ ] `next build` completes with no type errors, and the deployed app has no console warnings.

## Done check

The E2E suite passes; an admin edit shows up for customers and the audit log records who made it; a
non-admin is refused **server-side** on admin routes; a double-submitted order creates exactly one order;
a guest cart survives login without double-counting; an abandoned checkout releases its stock hold; a fast
search never renders a stale response; the LCP/CLS numbers are written down; the app stays usable when a
data call fails; optimistic updates roll back on error; axe is clean; no layout shift or hydration warnings.

**Stretch (optional):** coupon codes; multi-currency; a Stripe _test-mode_ checkout stub.
