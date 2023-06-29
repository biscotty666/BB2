## svelte.config.js

1. Added the `extensions` and `preprocess` blocks. Note that the `viteProcess()` was not specified by default as it was in the video. I added it here but don't know if that's necessary. It could be a version difference.
```
import adapter from '@sveltejs/adapter-auto'
import { vitePreprocess } from '@sveltejs/kit/vite'
import { mdsvex } from 'mdsvex'


/** @type {import('mdsvex').MdsvexOptions} */
const mdsvexOptions = {
	extensions: ['.md'],
}

/** @type {import('@sveltejs/kit').Config} */
const config = {
	extensions: ['.svelte','.md'],
	preprocess: [vitePreprocess(), mdsvex(mdsvexOptions)],
	kit: {
		adapter: adapter()
	}
}

export default config
```

---
up:: [[Svelte markdown blog]]
tags:: #note/example #effort/blog #source/video #source/JoyOfCode
