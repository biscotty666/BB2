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
tags:: #note/clearexample #effort/blog #source/video #source/JoyOfCode #on/Markdown 
