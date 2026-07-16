# Project — Blog

Build a blog with a public site, accounts with roles, and an editor dashboard. Mock
content, no real backend. This is your project for the month.

## Setup

- **Stack:** **Next.js (App Router)** + **TypeScript**, built **server-first** — pages are **Server
  Components** that read the mock content; add `'use client'` only where you need interactivity (search box,
  the editor).
- **Editor:** author posts in **[Tiptap](https://tiptap.dev/)** (rich text). Store the content as HTML and **render it sanitized** on the public site — never trust raw HTML.
- **Data:** mock posts in a module — **~15 posts**
  ([**`@faker-js/faker`**](https://www.npmjs.com/package/@faker-js/faker) recommended); Server Components
  read it directly:

  ```ts
  type Role = "viewer" | "editor" | "admin";

  interface User {
    id: string;
    email: string;
    name: string;
    role: Role;
  }

  interface Post {
    id: string;
    slug: string;
    title: string;
    excerpt: string;
    body: string; // rich HTML from the editor
    tags: string[];
    authorId: string;
    status: "draft" | "in_review" | "published";
    hidden: boolean; // admin toggle — public = published && !hidden
    publishedAt: string | null;
  }

  interface Comment {
    id: string;
    postId: string;
    authorId: string; // comments require a signed-in user
    body: string;
    createdAt: string;
    hidden: boolean; // admin toggle
  }
  ```

- **Accounts:** seed a few mock users — at least one **admin** (admins exist only in the seed data; nobody
  can sign up as one), plus some editors and viewers.
- **Responsive:** every screen works from mobile to desktop.

## Screens & routes

| Route                                                  | Screen           | Purpose                             |
| ------------------------------------------------------ | ---------------- | ----------------------------------- |
| `/`                                                    | Post list        | Browse, search, load more           |
| `/post/[slug]`                                         | Post detail      | Read a post                         |
| `/tag/[tag]`                                           | Tag listing      | Posts with a given tag              |
| `/post/[slug]/opengraph-image`                         | OG image         | Share image per post                |
| `/sitemap.xml`, `/rss.xml`, `/robots.txt`              | SEO endpoints    | Discoverability                     |
| `/signup`                                              | Sign up          | Create an account (editor checkbox) |
| `/login`                                               | Login            | Sign-in                             |
| `/dashboard`, `/dashboard/new`, `/dashboard/[id]/edit` | Editor dashboard | Manage your own posts               |
| `/dashboard/review`                                    | Approval queue   | Admin approves posts                |

## Feature specs

### Public site

- [ ] Post list: cards with title, excerpt, author, formatted date, tag chips, and a **reading time**
      computed from the body; sorted newest-first; a post is public only when it is **published and not
      hidden**.
- [ ] Client **search** by title (live, case-insensitive); "load more" (or pagination); empty state.
- [ ] Tag chips link to `/tag/[tag]`; the tag list is **derived from the data**, not hardcoded.
- [ ] Post detail renders the post content **sanitized** (headings, lists, links, images, **code blocks**) with title,
      author, date, tags, reading time; a bad slug shows "Post not found".

### Accounts & roles

- [ ] `/signup`: email + password + a **"Sign up as an editor"** checkbox — checked creates an `editor`,
      unchecked creates a `viewer`. Signing up sets a mock session.
- [ ] `/login` signs in against the mock users. The header shows who's signed in and offers sign out.
- [ ] Three roles: **viewer** (read + comment), **editor** (viewer + write their own posts), **admin**
      (approve posts, hide/show anything).

### Editor dashboard

- [ ] `/dashboard` is for editors and admins: a signed-out visitor goes to `/login`, a signed-in **viewer**
      is refused and sent back to `/`. It lists the signed-in editor's **own** posts with status.
- [ ] Create / edit a post in the **Tiptap** rich-text editor; save writes to the module.
- [ ] An editor moves their post **draft → in-review**, but **cannot publish it**. `/dashboard/review` is the
      **admin's** queue: the admin **approves** an in-review post, which publishes it.

### Comments

- [ ] **You must be signed in to comment** — a signed-out reader sees a prompt to sign in instead of the
      form. Any signed-in user (viewer, editor, or admin) can comment.
- [ ] Comments appear **immediately — there is no approval step**.

### Hide / show (admin)

- [ ] An admin can toggle **hide/show** on any post and any comment. Hidden items vanish from the public
      site; showing them puts them back.
- [ ] Hiding is **independent of status** — hiding a published post does not un-publish it, and showing it
      again needs no re-approval.

### SEO

- [ ] A basic `sitemap.xml`, `rss.xml`, `robots.txt`, and a per-post share image at
      `/post/[slug]/opengraph-image`. Hidden and unpublished posts stay out of the sitemap and feed.

### Global

- [ ] Dates formatted with `Intl`; readable typography; responsive; zero console warnings.

## Done check

Browsing list → post → tag works; post content renders correctly and sanitized (including code blocks);
search + tag filter + load-more behave together; signing up with the editor box checked lands you in the
dashboard while an unchecked signup does not; an editor can write a post and submit it, and only the admin
can approve it; a signed-out reader cannot comment; hiding a published post removes it from the public site
and showing it brings it back; a bad slug shows a not-found state.

**Stretch (optional):** a table of contents from headings; dark mode; "related posts" by shared tags.
