# Flow — Blog · Senior

Screen / user flow for the build.

```mermaid
flowchart TD
  Login["/login"] --> Role{"Role?"}
  Role -- author --> A["Write draft → submit for review"]
  Role -- editor --> E["Review queue: approve / publish"]
  Role -- admin --> Ad["Full access"]
  A --> Q["/dashboard/review"]
  E --> Q
  Q --> P{"Publish?"}
  P -- author forces --> F403["Refused server-side (author can't publish)"]
  P -- editor --> Rev["Publish → on-demand revalidate"] --> Live["Public site updated now"]
  Live --> SEO["Metadata + dynamic OG + JSON-LD + sitemap/rss"]
  Live --> Cm["Comments → editor approves / hides"]
```

Authorization is server-side: an author can submit but not publish; a forced publish is refused. Publishing
uses on-demand revalidation so the public site updates immediately.
