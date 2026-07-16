# Evaluation — Blog · Middle

## Specs

- [ ] Post pages SSG (`generateStaticParams`) + ISR (`revalidate`)
- [ ] `generateMetadata` per post (title/description/OG); a per-post OG image
- [ ] Only published posts public; a draft slug 404s
- [ ] Login/session; dashboard lists the author's posts
- [ ] Tiptap editor; save via Server Action; draft → in-review → publish triggers revalidation; review queue
- [ ] Comments via Server Action, optimistic
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
