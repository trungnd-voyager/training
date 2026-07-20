# Evaluation — Blog · Junior

## Specs

- [ ] Post list: title, excerpt, author, date, tags, reading time; newest-first; public = published && !hidden
- [ ] Search by title; load more; empty state
- [ ] Tag pages, with tags derived from the data
- [ ] Post page renders the content sanitized (headings, lists, links, images, code) + "not found"
- [ ] Signup with the editor checkbox (editor vs viewer); login; admin seeded only; header shows session
- [ ] Dashboard (editor/admin) lists the editor's own posts with status; viewers kept out
- [ ] Tiptap editor; editor submits draft → in-review but cannot publish; admin approves from the queue
- [ ] Must be signed in to comment; comments appear immediately with no approval
- [ ] Admin toggles hide/show on any post and any comment; hiding doesn't reset approval
- [ ] Basic sitemap/rss/robots + per-post OG image; hidden/unpublished posts excluded
- [ ] Dates via `Intl`; responsive; no console warnings
- [ ] Deployed; link works

## Code quality

- [ ] Strict TypeScript
- [ ] Server Components read the data; `'use client'` only where needed
- [ ] Content rendered sanitized (never trust raw HTML)

## Workflow

- [ ] Talked through the flow before diving in
- [ ] Asks good questions instead of guessing
- [ ] Can explain any line
- [ ] Bonus: did a Stretch item

## Verdict

- [ ] Right at Junior
- [ ] Ahead
- [ ] Not there yet
