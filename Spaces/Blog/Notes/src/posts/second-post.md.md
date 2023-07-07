## second-post.md

[[mdsvex]] will make the [[Frontmatter]] available as fields.

I had a little trouble with the favicon.png. NB. References to items in the `/static` folder should be called directly without prepending `/static`.

```
---
title: Second post
description: Second post.
date: '2023-06-25'
categories:
  - sveltekit
  - svelte
published: true
---

## Svelte

Media inside the **static** folder is served from '/'.

![Svelte](/favicon.png)
```

Updated with counter component

```
---
title: Second post
description: Second post.
date: '2023-06-25'
categories:
  - sveltekit
  - svelte
published: true
---
<script>
  import Counter from './counter.svelte'
</script>

## Table of Contents

## Svelte

Media inside the **static** folder is served from '/'.

![Svelte](/favicon.png)

## Counter

<Counter />
```


---
up:: [[Svelte markdown blog]]
tags:: #note/clearexample #effort/blog #source/video #source/JoyOfCode #on/Markdown 
