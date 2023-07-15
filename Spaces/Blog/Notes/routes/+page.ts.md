## +page.ts

```
import type { Post } from '$lib/types'
export async function load({ fetch }) {
  const response = await fetch('api/posts')
  const posts: Post[] = await response.json()
  return { posts }
}
```

---
up:: [[Svelte markdown blog]]
tags:: #note/clearexample #effort/blog #source/video #source/JoyOfCode #on/Svelte  
