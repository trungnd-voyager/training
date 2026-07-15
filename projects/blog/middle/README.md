# Project — Blog

Build a blog on **Next.js (App Router)** — static generation, an author dashboard,
and comments. Mock content, no real backend. This is your project for the month.

## Setup

- **Stack:** Next.js (App Router) + **TypeScript**, end to end.
- **Content:** posts in a mock DB via **route handlers** (or MDX files). Render markdown/MDX with a library.
  **~15 posts** ([**`@faker-js/faker`**](https://www.npmjs.com/package/@faker-js/faker) recommended) —
  `slug, title, excerpt, body, tags[], authorId, status, publishedAt`. Mock users for authors.
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
| `/dashboard`, `/dashboard/new`, `/dashboard/[id]/edit` | Author dashboard | Manage your posts                     |
| `/dashboard/review`                                    | Review queue     | Posts awaiting review                 |
| `/login`                                               | Login            | Session sign-in                       |

## Feature specs

### Public site (rendering)

- [ ] Post pages are **statically generated** with `generateStaticParams`, and use **ISR** (`revalidate`) so
      new/edited posts appear without a rebuild.
- [ ] Each post sets `generateMetadata`: title, description, Open Graph tags; a per-post OG image.
- [ ] Only **published** posts are public; a draft slug 404s publicly.

### Author dashboard (auth)

- [ ] Login (mock authors) sets a session; `/dashboard` lists the author's own posts with status.
- [ ] Create / edit a post in a markdown/MDX editor with **live preview**; save via a **Server Action**.
- [ ] Draft → in-review → publish status control; publishing makes the post public (triggering revalidation);
      `/dashboard/review` lists posts awaiting review.

### Comments

- [ ] Readers add comments on a post via a **Server Action**, shown **optimistically**.

### Framework

- [ ] `loading` / `error` / `not-found`; `notFound()` for a bad or unpublished slug.
- [ ] A deliberate caching/revalidation choice you can explain; `sitemap.xml` / `rss.xml` / `robots.txt`.
- [ ] Tests: unit tests for helpers (slug, excerpt, reading time) + a couple of component tests.

## Done check

A post published in the dashboard appears publicly after revalidation; drafts are not public; a post's
metadata/OG tags are correct (check the page source); comments post optimistically; tests pass.

**Stretch (optional):** MDX with custom components; a related-posts section; scheduled publishing.
