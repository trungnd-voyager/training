# Evaluation — Blog · Senior

## Specs

- [ ] SSG + on-demand revalidation on publish (change is live immediately, not on a timer)
- [ ] Can justify per route: SSG vs ISR vs dynamic
- [ ] `generateMetadata` + dynamic OG via `next/og`; sitemap/rss/robots + JSON-LD `Article`
- [ ] Workflow draft → in_review → published; roles author / editor / admin
- [ ] Server-side authz: an author can't publish; an editor can; a forced author-publish is refused
- [ ] Review queue; comment moderation (only approved comments public)
- [ ] Server-side search reflected in the URL
- [ ] Tests + Playwright E2E (author draft → editor publishes → public; unpublished not reachable)

## Code quality

- [ ] Strict TypeScript
- [ ] Authorization enforced both client and server-side
- [ ] Revalidation targeted (tags/paths)
- [ ] Unit + integration tests

## Workflow

- [ ] Talked through the flow before diving in
- [ ] Weighs trade-offs (caching, SEO, workflow) on their own
- [ ] Can explain any line
- [ ] Bonus: did a Stretch item

## Verdict

- [ ] Right at Senior
- [ ] Below Senior
