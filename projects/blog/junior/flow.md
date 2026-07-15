# Flow — Blog · Junior

Screen / user flow for the build.

```mermaid
flowchart TD
  Home["/"] --> S{"loading / empty / ok?"}
  S -- ok --> List["Published posts: search, tags, load more"]
  List --> Post["/post/[slug]"]
  Post --> V{"slug valid?"}
  V -- no --> NF["Post not found"]
  V -- yes --> Render["Render markdown + reading time"]
  Render --> Cm["Add comment (shown immediately)"]
  List --> Tag["/tag/[tag]"]
  Home --> Login["/login"] --> Dash["/dashboard: my posts + status"]
  Dash --> Edit["New / Edit: editor + live preview"] --> Save["Save to module"]
  Edit --> St["draft → in-review → published"]
  Dash --> Rev["/dashboard/review"]
```
