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
tags:: #note/clearexample #effort/blog #source/video #source/JoyOfCode #on/Markdown 
