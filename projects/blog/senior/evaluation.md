# Evaluation — Blog · Senior

## Specs

- [ ] SSG + on-demand revalidation on approve and on hide/show (live immediately, not on a timer)
- [ ] Can justify per route: SSG vs ISR vs dynamic
- [ ] `generateMetadata` + dynamic OG via `next/og`; sitemap/rss/robots + JSON-LD `Article`
- [ ] Signup editor checkbox; server assigns the role and refuses a signup requesting `admin`
- [ ] Workflow draft → in_review → published; roles viewer / editor / admin; middleware gating
- [ ] Server-side authz: an editor can't approve; only an admin can; a forged editor-approve is refused
- [ ] Admin approval queue; sign-in required to comment (rejected server-side if not); no approval step
- [ ] Admin-only hide/show on posts and comments, authorized server-side; public = published && !hidden
- [ ] Server-side search reflected in the URL
- [ ] Tests + Playwright E2E (editor draft → admin approves → public; unpublished/hidden not reachable)
- [ ] Deployed; link works

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
