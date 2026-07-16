# Flow — Blog · Middle

Screen / user flow for the build.

```mermaid
flowchart TD
  Build["Build: generateStaticParams (published)"] --> Pub["/ , /post/[slug]"]
  Pub --> Vis{"published?"}
  Vis -- no --> NF["404"]
  Vis -- yes --> Read["Read post + generateMetadata / OG"]
  Read --> Cm["Comment"]
  Login["/login"] --> Dash["/dashboard (auth)"]
  Dash --> Ed["Editor"] --> SA["Save"]
  SA --> Publish["Publish → revalidate"] --> Pub
  Dash --> Rev["/dashboard/review"]
```

Public pages are statically generated and revalidated (ISR). Publishing from the dashboard triggers
revalidation so the change shows up without a rebuild.
