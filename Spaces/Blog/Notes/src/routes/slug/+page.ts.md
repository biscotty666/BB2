## +page.ts

```
import { error } from '@sveltejs/kit'

export async function load({params}) {
  try {
    const post = await import(`../../posts/${params.slug}.md`)
    return {
      content: post.default,
      meta: post.metadata
    }
  } catch (e) {
    throw error(404, `Could not find ${params.slug}`)
  }
}

```

---
up:: [[Svelte markdown blog]]
tags:: #note/example #effort/blog #source/video #on/Svelte 
Source:: [[Joy of Code]]

