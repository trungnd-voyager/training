# Flow — Blog · Senior

Screen / user flow for the build.

```mermaid
flowchart TD
  Signup["/signup + 'editor' checkbox"] --> SvR{"server assigns role"}
  SvR -- checkbox set --> Ed0["editor"]
  SvR -- checkbox unset --> Vw0["viewer"]
  SvR -- admin requested --> S403["Refused — admins are seeded only"]
  Login["/login"] --> Role{"Role?"}
  Ed0 --> Role
  Vw0 --> Role

  Role -- viewer --> V["Read + comment"]
  Role -- editor --> A["Write draft → submit for review"]
  Role -- admin --> Ad["Full access"]

  A --> Q["/dashboard/review (admin queue)"]
  Ad --> Q
  Q --> P{"Approve?"}
  P -- editor forges --> F403["Refused server-side (editor can't approve)"]
  P -- admin --> Rev["Approve → published → on-demand revalidate"] --> Live["Public site updated now"]
  Live --> SEO["Metadata + dynamic OG + JSON-LD + sitemap/rss"]

  Live --> CmG{"session?"}
  CmG -- no --> C401["Comment rejected server-side"]
  CmG -- yes --> Cm["Comment is public immediately (no approval)"]

  Ad --> Tog["Toggle hide/show: any post or comment"]
  Tog --> Vis{"published && !hidden?"}
  Vis -- no --> Gone["Not public (approval retained)"]
  Vis -- yes --> Live
```

Authorization is server-side throughout: the signup checkbox is a hint, not a trust boundary, and an admin
role can never be requested; an editor can submit but not approve, and a forged approve is refused; a
signed-out comment is rejected. Approving and hiding both use on-demand revalidation so the public site
updates immediately. Hide/show is orthogonal to status — hiding a published post keeps its approval.
