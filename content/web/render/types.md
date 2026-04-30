---
title: "Types of rendering - next.js"
tags: ["web", "rendering"]
draft: true
---

Specific limitations and caracteristics of the different types of rendering inside next.js


# Server components ( default in next.js )

1. Rendered on the server at request time
2. Can access secure data directly ( cookies, database ) without risk
3. Cannot use React hooks like *useState* or *useEffect*

# Client Components

1. Can use different types of hooks
2. Render on the client
3. Cannot access server-only API'directly

