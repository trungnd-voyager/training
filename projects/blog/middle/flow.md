# Flow — Blog · Middle

Screen / user flow for the build.

```mermaid
flowchart TD
  Build["Build: generateStaticParams (published && !hidden)"] --> Pub["/ , /post/[slug]"]
  Pub --> Vis{"published && !hidden?"}
  Vis -- no --> NF["404"]
  Vis -- yes --> Read["Read post + generateMetadata / OG"]
  Read --> Gate{"signed in?"}
  Gate -- no --> Prompt["Prompt to sign in"]
  Gate -- yes --> Cm["Comment (optimistic, no approval)"]

  Signup["/signup + 'editor' checkbox"] --> Role{"Role?"}
  Login["/login (admin seeded only)"] --> Role
  Role -- viewer --> Read
  Role -- editor --> Dash["/dashboard (own posts)"]
  Role -- admin --> Rev["/dashboard/review (approval queue)"]

  Dash --> Ed["Tiptap editor"] --> SA["Save (onSubmit → route handler)"]
  SA --> Sub["Submit: draft → in_review"] --> Rev
  Rev --> Ap["Admin approves → published → revalidate"] --> Pub
  Role -- admin --> Tog["Toggle hide/show (post or comment) → revalidate"] --> Pub
```

Public pages are statically generated and revalidated (ISR); a post is public only when published and not
hidden. Signup picks viewer or editor by checkbox; admins are seeded. An editor submits for review but only
an admin approves, and only an admin toggles hide/show — both trigger revalidation so the public site
reflects the change without a rebuild.
