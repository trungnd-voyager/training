# Project — Blog

Build a production-grade publishing platform on **Next.js (App Router)** — full SEO,
an editorial workflow with roles, on-demand revalidation, and a real test suite. Mock content, no real
backend. This is your project for the month.

## Setup

- **Stack:** Next.js (App Router) + **TypeScript**, end to end.
- **Content:** posts in a mock DB via **route handlers** (or MDX)
  ([**`@faker-js/faker`**](https://www.npmjs.com/package/@faker-js/faker) recommended) —
  `slug, title, excerpt, body, tags[], authorId, status, publishedAt`. Mock users with a `role` of
  `author` / `editor` / `admin`.
- **Testing:** Vitest + React Testing Library + **Playwright** (E2E).
- **Responsive:** every screen works from mobile to desktop.

## Screens & routes

| Route                                                    | Notes                                       |
| -------------------------------------------------------- | ------------------------------------------- |
| `/` , `/post/[slug]` , `/tag/[tag]`                      | Public site; SSG + on-demand revalidation   |
| `/post/[slug]/opengraph-image`                           | **Dynamically generated** OG image per post |
| `/sitemap.xml` , `/rss.xml` , `/robots.txt`              | SEO endpoints                               |
| `/dashboard` , `/dashboard/new` , `/dashboard/[id]/edit` | Editorial dashboard (role-gated)            |
| `/dashboard/review`                                      | Editor's queue of posts awaiting review     |
| `/login`                                                 | Session login                               |

## Feature specs

### Rendering & performance

- [ ] Public pages are statically generated; on **publish**, use **on-demand revalidation**
      (`revalidateTag` / `revalidatePath`) so the change is live immediately — not just on a timer.
- [ ] You can articulate, per route, why it's SSG vs ISR vs dynamic.

### SEO (the real thing)

- [ ] `generateMetadata` per post; a **dynamic OG image** per post via `next/og` (`ImageResponse`).
- [ ] `sitemap.xml`, `rss.xml`, `robots.txt`, and **JSON-LD** `Article` structured data on posts.

### Editorial workflow + roles

- [ ] Statuses `draft` → `in_review` → `published`. Roles `author` / `editor` / `admin`.
- [ ] Enforced with **server-side authorization**: an author can submit their own post for review but
      **cannot publish**; an editor can publish; a forged request from an author to publish is refused.
- [ ] `/dashboard/review` is the editor's queue.

### Comments with moderation

- [ ] Readers comment via a Server Action; an editor can **approve / hide** comments; only approved comments
      show publicly.

### Search

- [ ] Server-side search across posts (title + body), reflected in the URL.

### Testing

- [ ] Meaningful unit + integration tests (authorization, revalidation, workflow transitions).
- [ ] **At least one Playwright E2E:** author writes a draft → editor publishes → it appears publicly; an
      unpublished post is not publicly reachable.

## Done check

Publishing updates the public site immediately (on-demand revalidation); each post has correct metadata and a
generated OG image; an author cannot publish server-side; unapproved comments stay hidden; the E2E passes.

**Stretch (optional):** scheduled publishing; post analytics; multi-author series/collections.
