# Project — E-commerce Storefront

Build a working online storefront for a small shop — catalog, cart, checkout, and a
small admin. Mock data, no real payments. This is your project for the month.

## Setup

- **Stack:** **Next.js (App Router)** + **TypeScript**, built **server-first** — pages are **Server
  Components** that read the mock data on the server; add `'use client'` only on interactive leaves
  (search box, cart, quantity selector). Avoid `any`.
- **Data:** no real backend. Mock data in `src/data/products.ts` — **~40 products**
  ([**`@faker-js/faker`**](https://www.npmjs.com/package/@faker-js/faker) recommended); Server Components
  import it directly:

  ```ts
  interface Product {
    id: string;
    slug: string;
    name: string;
    priceCents: number; // integer cents
    category: string; // Keyboards | Mice | Monitors | Cables | Audio
    description: string;
    images: string[];
    stock: number;
    rating: number;
  }
  ```

- **Responsive:** every screen works from mobile (2-col grid) to desktop (4-col).

## Screens & routes

| Route                                          | Screen         | Purpose                          |
| ---------------------------------------------- | -------------- | -------------------------------- |
| `/`                                            | Catalog        | Browse, filter, search, sort     |
| `/product/[slug]`                              | Product detail | One product; quick-view from `/` |
| `/category/[slug]`                             | Category       | Products in one category         |
| `/cart`                                        | Cart           | Review items, change quantities  |
| `/checkout`                                    | Checkout       | Shipping form, place order       |
| `/checkout/success`                            | Confirmation   | Order number + summary           |
| `/login`                                       | Login          | Sign-in                          |
| `/admin/products`, `/admin/products/[id]/edit` | Admin          | Manage products                  |

**Shared header** on every screen: shop logo (→ `/`) and a live cart-count badge.

## Feature specs

### Catalog (`/`)

- [ ] Server-rendered responsive grid; card: image, name, price, category, out-of-stock badge.
- [ ] **Category filter** derived from the data (not hardcoded); client **search** (live, case-insensitive);
      **sort** by price and name; filter + search + sort work together; result count; empty state.

### Product + quick-view

- [ ] Product page: images, name, price, description, rating, stock, quantity + "Add to cart" (disabled when
      out of stock); a bad slug shows "Product not found".
- [ ] Clicking a product on the catalog opens a **quick-view modal** (a client modal is fine here).

### Category (`/category/[slug]`)

- [ ] Lists the products in one category.

### Cart & checkout

- [ ] Cart in a client Context, persisted to **localStorage**: line items, inline quantity edit, subtotal +
      shipping + total, empty state; the header count updates live.
- [ ] Checkout: **controlled** form with per-field validation + order summary; "Place order" disabled until
      valid → `/checkout/success`, cart cleared. Success shows a mock order number; an empty-cart direct
      visit redirects to `/`.

### Admin (`/admin/products`)

- [ ] A products table + create/edit form (client CRUD against the module), behind a **mock login** gate at
      `/login`.

### Global

- [ ] Prices via `Intl.NumberFormat` from integer cents; loading skeletons; responsive; zero console warnings.

## Done check

Catalog → product → cart → checkout → confirmation runs with no dead ends; pages render as Server
Components; cart math is correct and survives reload; the admin can add and edit a product.

**Stretch (optional):** wishlist; a recently-viewed row; product reviews UI.
