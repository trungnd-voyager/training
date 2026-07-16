# Evaluation — Blog · Middle

## Specs

- [ ] Post pages SSG (`generateStaticParams`) + ISR (`revalidate`)
- [ ] `generateMetadata` per post (title/description/OG); a per-post OG image
- [ ] Public = published && !hidden; a draft or hidden slug 404s
- [ ] Signup with the editor checkbox (editor vs viewer); login/session; admin seeded only
- [ ] Dashboard gated to editor/admin, lists the editor's own posts
- [ ] Tiptap editor; save via `onSubmit` → route handler; editor submits draft → in-review, cannot publish
- [ ] Admin approves from the queue → published + revalidation
- [ ] Comments via `onSubmit` → route handler, optimistic; sign-in required; no approval step
- [ ] Admin toggles hide/show on any post/comment → revalidation; hiding keeps approval intact
- [ ] loading/error/not-found; a caching/revalidation choice they can explain; sitemap/rss/robots
- [ ] Tests: helpers + a couple of components

## Code quality

- [ ] Strict TypeScript
- [ ] Server-first; `'use client'` only on leaves
- [ ] Clean data access via route handlers / module

## Workflow

- [ ] Talked through the flow before diving in
- [ ] Can explain the rendering choices (SSG vs ISR) and why
- [ ] Can explain any line
- [ ] Bonus: did a Stretch item

## Verdict

- [ ] Right at Middle
- [ ] Ahead
- [ ] Not there yet
