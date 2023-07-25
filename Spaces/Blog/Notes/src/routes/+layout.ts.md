## +layout.ts

```
export const prerender = true

export async function load({ url }) {
  return {
    url: url.pathname
  }
}
```

To pre-render as a static site
```
export const prerender = true

export async function load({ url }) {
  return {
    url: url.pathname
  }
}
```

---
up:: [[Svelte markdown blog]]
tags:: #note/example #effort/blog #source/video #on/Svelte  
