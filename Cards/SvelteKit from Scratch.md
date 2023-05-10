up:: [[SvelteKit Course]]
tags:: #note/reference #on/Svelte 
X:: 

## SvelteKit from Scratch


```
	- pnpm i -D vite @sveltejs/kit @sveltejs/adapter-auto svelte
```

`package.json`:

```
{
  "type": "module",
  "scripts": {
    "dev": "vite dev",
    "build": "vite build",
    "preview": "vite preview"
  },
  "devDependencies": {
    "@sveltejs/adapter-auto": "^2.0.0",
    "@sveltejs/kit": "^1.15.1",
    "svelte": "^3.58.0",
    "vite": "^4.2.1"
  }
}
```

`vite.config.js`:

```
import { sveltekit } from "@sveltejs/kit/vite";
/** @type {import("vite").UserConfig} */
const config = {
plugins: [sveltekit()]
}
export default config
```

`svelte.config.js`

```
import adapter from '@sveltejs/adapter-auto'
import { vitePreprocess } from '@sveltejs/kit/vite'

/** @type {import("@sveltejs/kit").Config} */

const config = {
	preprocess: vitePreprocess(),
	kit: {
		adapter: adapter()
	},
}

export default config
```

`src/app.html`

```
%3Chead%3E
	%sveltekit.head%
</head>
<body>
	%sveltekit.body%
</body
```

`src/routes/+page.svelte`
```
<h1>Jello World</h1>
```

`pnpm run dev`>)

---
### References

