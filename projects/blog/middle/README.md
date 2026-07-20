# Project — Blog

Build a blog on **Next.js (App Router)** — static generation, accounts with roles, an
editor dashboard, and comments. Mock content, no real backend. This is your project for the month.

## Setup

- **Stack:** Next.js (App Router) + **TypeScript**, end to end.
- **Content:** posts in a mock DB via **route handlers**; author in **[Tiptap](https://tiptap.dev/)** (store HTML, render sanitized).
  **~15 posts** ([**`@faker-js/faker`**](https://www.npmjs.com/package/@faker-js/faker) recommended) —
  `slug, title, excerpt, body, tags[], authorId, status, hidden, publishedAt`. Comments carry
  `postId, authorId, body, createdAt, hidden`.
- **Accounts:** mock users with a `role` of `viewer` / `editor` / `admin`. Seed at least one **admin** —
  admins exist only in the seed data and cannot be created by signup.
- **Testing:** Vitest + React Testing Library.
- **Responsive:** every screen works from mobile to desktop.

## Screens & routes

| Route                                                  | Screen           | Purpose                               |
| ------------------------------------------------------ | ---------------- | ------------------------------------- |
| `/`                                                    | Post list        | Statically generated, ISR             |
| `/post/[slug]`                                         | Post detail      | `generateStaticParams` + `revalidate` |
| `/tag/[tag]`                                           | Tag listing      | Posts with a given tag                |
| `/post/[slug]/opengraph-image`                         | OG image         | Share image per post                  |
| `/sitemap.xml`, `/rss.xml`, `/robots.txt`              | SEO endpoints    | Discoverability                       |
| `/dashboard`, `/dashboard/new`, `/dashboard/[id]/edit` | Editor dashboard | Manage your own posts                 |
| `/dashboard/review`                                    | Approval queue   | Admin approves posts                  |
| `/signup`                                              | Sign up          | Create an account (editor checkbox)   |
| `/login`                                               | Login            | Session sign-in                       |

## Feature specs

### Public site (rendering)

- [ ] Post pages are **statically generated** with `generateStaticParams`, and use **ISR** (`revalidate`) so
      new/edited posts appear without a rebuild.
- [ ] Each post sets `generateMetadata`: title, description, Open Graph tags; a per-post OG image.
- [ ] A post is public only when **published and not hidden**; a draft or hidden slug 404s publicly.

### Accounts & roles

- [ ] `/signup`: email + password + a **"Sign up as an editor"** checkbox — checked creates an `editor`,
      unchecked a `viewer`; it sets a session. `/login` signs in an existing mock user.
- [ ] Roles: **viewer** (read + comment), **editor** (viewer + write their own posts), **admin** (approve
      posts, hide/show anything). `/dashboard` is gated to editor/admin.

### Editor dashboard

- [ ] `/dashboard` lists the signed-in editor's **own** posts with status.
- [ ] Create / edit a post in the **Tiptap** rich-text editor; save via an **`onSubmit` handler** POSTing to a
      route handler.
- [ ] An editor moves a post **draft → in-review** but **cannot publish**. `/dashboard/review` is the
      **admin's** approval queue; the admin approving an in-review post publishes it and **triggers
      revalidation** so it appears publicly.

### Comments

- [ ] **Signed-in users only** — a signed-out reader gets a prompt to sign in, not a form. Any role can
      comment.
- [ ] Comments post via an **`onSubmit` handler** to a route handler, shown **optimistically** and reconciled
      on the response. **No approval step.**

### Hide / show (admin)

- [ ] An admin toggles **hide/show** on any post and any comment through a route handler; hidden items leave
      the public site, and hiding a post **revalidates** the affected pages.
- [ ] Hiding is **independent of status** — a hidden published post keeps its approval and needs none to
      come back.

### Framework

- [ ] `loading` / `error` / `not-found`; `notFound()` for a bad or unpublished slug.
- [ ] A deliberate caching/revalidation choice you can explain; `sitemap.xml` / `rss.xml` / `robots.txt`.
- [ ] Tests: unit tests for helpers (slug, excerpt, reading time) + a couple of component tests.

## Deploy

- [ ] Deployed to Vercel; the live link is in the repo README.
- [ ] `next build` completes with no type errors, and the deployed app has no console warnings.

## Done check

A post the admin approves appears publicly after revalidation; drafts and hidden posts are not public; an
editor has no way to publish their own post; a signed-out reader cannot comment; hiding a comment removes it
for everyone; a post's metadata/OG tags are correct (check the page source); comments post optimistically;
tests pass.

**Stretch (optional):** custom Tiptap extensions (embeds, callouts); a related-posts section; scheduled publishing.
