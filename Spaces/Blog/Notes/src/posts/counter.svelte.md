## counter.svelte

```
<script lang="ts">
  let count = 0
  const increment = () => (count += 1)
</script>

<button on:click={increment}>
  {count}
</button>
```

---
up:: [[Svelte markdown blog]]
tags:: #note/example #effort/blog #source/video  #on/Svelte 
Source:: [[Joy of Code]]

