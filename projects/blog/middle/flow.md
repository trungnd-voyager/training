# Flow — Blog · Middle

Screen / user flow for the build.

```mermaid
flowchart TD
  Build["Build: generateStaticParams (published)"] --> Pub["/ , /post/[slug] — SSG + ISR"]
  Pub --> Vis{"published?"}
  Vis -- no --> NF["404 — draft not public"]
  Vis -- yes --> Read["Read post + generateMetadata / OG"]
  Read --> Cm["Comment — Server Action, optimistic"]
  Login["/login"] --> Dash["/dashboard (auth)"]
  Dash --> Ed["Editor + live preview"] --> SA["Save — Server Action"]
  SA --> Publish["Publish → revalidate"] --> Pub
  Dash --> Rev["/dashboard/review"]
```

Public pages are statically generated and revalidated (ISR). Publishing from the dashboard triggers
revalidation so the change shows up without a rebuild.
