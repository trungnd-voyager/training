# Flow — Blog · Junior

Screen / user flow for the build.

```mermaid
flowchart TD
  Home["/"] --> S{"loading / empty / ok?"}
  S -- ok --> List["Public posts: published && !hidden — search, tags, load more"]
  List --> Post["/post/[slug]"]
  Post --> V{"slug valid?"}
  V -- no --> NF["404 - Post not found"]
  V -- yes --> Render["Render content + reading time"]
  Render --> CmGate{"signed in?"}
  CmGate -- no --> Prompt["Prompt to sign in — no comment form"]
  CmGate -- yes --> Cm["Add comment (immediate, no approval)"]
  List --> Tag["/tag/[tag]"]

  Home --> Signup["/signup: email + password + 'editor' checkbox"]
  Signup --> Chk{"editor checked?"}
  Chk -- yes --> RE["role = editor"]
  Chk -- no --> RV["role = viewer"]
  Home --> Login["/login (seeded admin lives here only)"]
  Login --> Role{"Role?"}
  RE --> Role
  RV --> Role

  Role -- viewer --> Read["Read + comment"]
  Role -- editor --> Dash["/dashboard: my posts + status"]
  Role -- admin --> Rev["/dashboard/review: approval queue"]

  Dash --> Edit["New / Edit: Tiptap editor"] --> Save["Save to module"]
  Edit --> Sub["draft → in-review (editor cannot publish)"] --> Rev
  Rev --> Ap["Admin approves → published"] --> List
  Role -- admin --> Tog["Toggle hide/show on any post or comment"] --> List
```

Signup picks the role via a checkbox — editor or viewer; admins are seeded only. Commenting requires a
sign-in but no approval. Only an admin approves posts, and only an admin toggles hide/show, which is
independent of the draft → in-review → published status.
