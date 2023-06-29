## transition.svelte

```
<script lang="ts">
  import { fade } from "svelte/transition";
  export let url = ''
</script>

{#key url}
  <div class="transition" in:fade>
    <slot />
  </div>
{/key}

<style>
  .transition {
    height: 100%;
  }
</style>
```

---
up:: [[Svelte markdown blog]]
tags:: #note/example #effort/blog #source/video #source/JoyOfCode #on/Markdown 
