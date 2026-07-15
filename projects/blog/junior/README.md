# Project — Blog

Build a blog with a public site and an author dashboard. Mock content, no real
backend. This is your project for the month.

## Setup

- **Stack:** **Next.js (App Router)** + **TypeScript**, built **server-first** — pages are **Server
  Components** that read the mock content; add `'use client'` only where you need interactivity (search box,
  editor preview).
- **Markdown:** render post bodies with a library (e.g. `react-markdown`). Don't hand-parse markdown.
- **Data:** mock posts in a module — **~15 posts**
  ([**`@faker-js/faker`**](https://www.npmjs.com/package/@faker-js/faker) recommended); Server Components
  read it directly:

  ```ts
  interface Post {
    id: string;
    slug: string;
    title: string;
    excerpt: string;
    body: string; // markdown
    tags: string[];
    authorId: string;
    status: "draft" | "in_review" | "published";
    publishedAt: string | null;
  }
  ```

- **Responsive:** every screen works from mobile to desktop.

## Screens & routes

| Route                                                  | Screen           | Purpose                   |
| ------------------------------------------------------ | ---------------- | ------------------------- |
| `/`                                                    | Post list        | Browse, search, load more |
| `/post/[slug]`                                         | Post detail      | Read a post (markdown)    |
| `/tag/[tag]`                                           | Tag listing      | Posts with a given tag    |
| `/post/[slug]/opengraph-image`                         | OG image         | Share image per post      |
| `/sitemap.xml`, `/rss.xml`, `/robots.txt`              | SEO endpoints    | Discoverability           |
| `/dashboard`, `/dashboard/new`, `/dashboard/[id]/edit` | Author dashboard | Manage your posts         |
| `/dashboard/review`                                    | Review queue     | Posts awaiting review     |
| `/login`                                               | Login            | Sign-in                   |

## Feature specs

### Public site

- [ ] Post list: cards with title, excerpt, author, formatted date, tag chips, and a **reading time**
      computed from the body; sorted newest-first; only **published** posts are public.
- [ ] Client **search** by title (live, case-insensitive); "load more" (or pagination); empty state.
- [ ] Tag chips link to `/tag/[tag]`; the tag list is **derived from the data**, not hardcoded.
- [ ] Post detail renders markdown properly (headings, lists, links, images, **code blocks**) with title,
      author, date, tags, reading time; a bad slug shows "Post not found".

### Author dashboard

- [ ] A **mock login** gate at `/login`; `/dashboard` lists the author's own posts with status.
- [ ] Create / edit a post in a markdown editor with **live preview**; save writes to the module.
- [ ] A draft → in-review → published status control; `/dashboard/review` lists posts awaiting review.

### Comments & SEO

- [ ] Readers add comments on a post (kept in local/module state), shown immediately.
- [ ] A basic `sitemap.xml`, `rss.xml`, `robots.txt`, and a per-post share image at
      `/post/[slug]/opengraph-image`.

### Global

- [ ] Dates formatted with `Intl`; readable typography; responsive; zero console warnings.

## Done check

Browsing list → post → tag works; markdown renders correctly (including code blocks); search + tag filter +
load-more behave together; the author can write a post and move it through draft → review → published; a
bad slug shows a not-found state.

**Stretch (optional):** a table of contents from headings; dark mode; "related posts" by shared tags.
