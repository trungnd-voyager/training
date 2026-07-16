# Project — Blog

Build a production-grade publishing platform on **Next.js (App Router)** — full SEO,
an editorial workflow with roles, on-demand revalidation, and a real test suite. Mock content, no real
backend. This is your project for the month.

## Setup

- **Stack:** Next.js (App Router) + **TypeScript**, end to end.
- **Content:** posts in a mock DB via **route handlers**; author in **[Tiptap](https://tiptap.dev/)** (store HTML, render sanitized)
  ([**`@faker-js/faker`**](https://www.npmjs.com/package/@faker-js/faker) recommended) —
  `slug, title, excerpt, body, tags[], authorId, status, hidden, publishedAt`. Comments carry
  `postId, authorId, body, createdAt, hidden`.
- **Accounts:** mock users with a `role` of `viewer` / `editor` / `admin`. Seed at least one **admin** —
  admins exist only in the seed data and cannot be created by signup.
- **Testing:** Vitest + React Testing Library + **Playwright** (E2E).
- **Responsive:** every screen works from mobile to desktop.

## Screens & routes

| Route                                                    | Notes                                       |
| -------------------------------------------------------- | ------------------------------------------- |
| `/` , `/post/[slug]` , `/tag/[tag]`                      | Public site; SSG + on-demand revalidation   |
| `/post/[slug]/opengraph-image`                           | **Dynamically generated** OG image per post |
| `/sitemap.xml` , `/rss.xml` , `/robots.txt`              | SEO endpoints                               |
| `/dashboard` , `/dashboard/new` , `/dashboard/[id]/edit` | Editorial dashboard (role-gated)            |
| `/dashboard/review`                                      | **Admin's** queue of posts awaiting approval |
| `/signup`                                                | Create an account (editor checkbox)         |
| `/login`                                                 | Session login                               |

## Feature specs

### Rendering & performance

- [ ] Public pages are statically generated; on **approve** and on **hide/show**, use **on-demand
      revalidation** (`revalidateTag` / `revalidatePath`) so the change is live immediately — not just on a
      timer.
- [ ] You can articulate, per route, why it's SSG vs ISR vs dynamic.

### SEO (the real thing)

- [ ] `generateMetadata` per post; a **dynamic OG image** per post via `next/og` (`ImageResponse`).
- [ ] `sitemap.xml`, `rss.xml`, `robots.txt`, and **JSON-LD** `Article` structured data on posts.

### Accounts & roles

- [ ] `/signup`: email + password + an **"editor" checkbox** — checked creates an `editor`, unchecked a
      `viewer`. **The checkbox is a client-side hint, not a trust boundary**: the server decides the role and
      **refuses any signup requesting `admin`**, however the request is crafted.
- [ ] `/login` sets a session; **middleware** gates `/dashboard` to editor/admin and `/dashboard/review` to
      admin.

### Editorial workflow + roles

- [ ] Statuses `draft` → `in_review` → `published`. Roles `viewer` / `editor` / `admin`.
- [ ] Enforced with **server-side authorization**: an editor can submit their own post for review but
      **cannot approve it**; only an admin approves; a forged approve request from an editor is refused.
- [ ] `/dashboard/review` is the **admin's** approval queue.

### Comments

- [ ] Comments require a **session** — the route handler rejects an unauthenticated comment even if the UI is
      bypassed. Any role may comment, and comments are public **immediately with no approval**.

### Hide / show (admin)

- [ ] An admin toggles **hide/show** on any post and any comment; public visibility is
      `published && !hidden`. The toggle is **authorized server-side** — a forged hide/show from an editor or
      viewer is refused.
- [ ] Hiding is orthogonal to status: a hidden published post retains its approval, and un-hiding requires no
      re-approval. Toggling triggers **on-demand revalidation**.

### Search

- [ ] Server-side search across posts (title + body), reflected in the URL.

### Testing

- [ ] Meaningful unit + integration tests (authorization, revalidation, workflow transitions).
- [ ] **At least one Playwright E2E:** an editor writes a draft → an admin approves → it appears publicly; an
      unpublished or hidden post is not publicly reachable.

## Done check

Approving updates the public site immediately (on-demand revalidation); each post has correct metadata and a
generated OG image; an editor cannot approve server-side and cannot sign up as an admin; a signed-out
comment POST is rejected; an admin's hide pulls a post or comment from the public site immediately; the E2E
passes.

**Stretch (optional):** scheduled publishing; post analytics; multi-author series/collections.
