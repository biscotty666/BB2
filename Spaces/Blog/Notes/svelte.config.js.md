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

2. Added [[Shiki]] stuff

```
import adapter from '@sveltejs/adapter-auto'
import { vitePreprocess } from '@sveltejs/kit/vite'
import { escapeSvelte, mdsvex } from 'mdsvex'
import shiki from 'shiki'


/** @type {import('mdsvex').MdsvexOptions} */
const mdsvexOptions = {
	extensions: ['.md'],
	highlight: {
		highlighter: async (code, lang ='text') => {
			const highlighter = await shiki.getHighlighter({
				theme: 'dracula-soft'})
			const html = escapeSvelte(highlighter.codeToHtml(code, {lang}))
			return `{@html \`${html}\`}`
		}
	}
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

Added mdsvex layout stuff

```
import adapter from '@sveltejs/adapter-vercel'
import { vitePreprocess } from '@sveltejs/kit/vite'
import { escapeSvelte, mdsvex } from 'mdsvex'
import shiki from 'shiki'

/** @type {import('mdsvex').MdsvexOptions} */
const mdsvexOptions = {
	extensions: ['.md'],
	layout: {
		_: './src/mdsvex.svelte'
	},
	highlight: {
		highlighter: async (code, lang ='text') => {
			const highlighter = await shiki.getHighlighter({
				theme: 'dracula-soft'})
			const html = escapeSvelte(highlighter.codeToHtml(code, {lang}))
			return `{@html \`${html}\`}`
		}
	},
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

Add remark/rehype. 

```
import adapter from '@sveltejs/adapter-vercel'
import { vitePreprocess } from '@sveltejs/kit/vite'
import { escapeSvelte, mdsvex } from 'mdsvex'
import shiki from 'shiki'
import remarkUnwrapImages from 'remark-unwrap-images'
import remarkToc from 'remark-toc'
import rehypeSlug from 'rehype-slug'

/** @type {import('mdsvex').MdsvexOptions} */
const mdsvexOptions = {
	extensions: ['.md'],
	layout: {
		_: './src/mdsvex.svelte'
	},
	highlight: {
		highlighter: async (code, lang ='text') => {
			const highlighter = await shiki.getHighlighter({
				theme: 'dracula-soft'})
			const html = escapeSvelte(highlighter.codeToHtml(code, {lang}))
			return `{@html \`${html}\`}`
		}
	},
	remarkPlugins: [remarkUnwrapImages, [remarkToc, { tight: true }]],
	rehypePlugins: [rehypeSlug]
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
tags:: #note/example #effort/blog #source/video 
Source:: [[Joy of Code]]
