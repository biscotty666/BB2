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
tags:: #note/clearexample #effort/blog #source/video #source/JoyOfCode #on/Svelte 
