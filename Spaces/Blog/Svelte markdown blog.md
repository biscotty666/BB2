up:: [[Svelte]]
tags:: #note/example #source/video #effort/blog #source/JoyOfCode #on/Markdown 

## Svelte markdown blog

1. Create new sveltekit project  `pnpm create svelte`
2. Install lucide, open-props and two fonts from fontsource
`pnpm i open-props lucide-svelte @fontsource/manrope @fontsource/jetbrans-mono`
3. Add the attribute `"semi": false` to `prettierrc`
4. Auto-format with `pnpm run format`
5. Add favicon link from fav.farm
6. Create config file [[config.ts]]
8. Create [[+layout.svelte]]
9. Create [[header.svelte]]
10. Create [[app.css]]
11. Install [[mdsvex]] as a development dependency `pnpm i -D mdsvex`
12. Edit [[Spaces/Blog/Notes/svelte.config.js|svelte.config.js]].
13. Create `src/posts` directory and two example notes [[first-post.md|first-post.md]] and  [[second-post.md|second-post.md]] in Markdown format.
14. To create a stand-alone api endpoint create the file [[Spaces/Blog/Notes/src/routes/api/posts/+server.ts|api/posts/+server.ts]] 
15. To set-up Typescript types for Posts and Categories create [[types.ts|types.ts]].
16. To provide the posts to the main page create [[+page.ts|+page.ts]].
17. Create [[+page.svelte|src/+page.svelte]].
18. Create a utility function in [[Spaces/Blog/Notes/src/lib/utils.ts|utils.ts]] to format dates.
19. Create [[Spaces/Blog/Notes/src/routes/slug/+page.svelte|routes/slug/+page.svelte]] and [[Spaces/Blog/Notes/src/routes/slug/+page.ts|routes/slug/+page.ts]]. 
	- The word _slug_ requires square brackets
	- The text written in slug will be available as `params.slug` in the `params` in the load function of [[Spaces/Blog/Notes/src/routes/slug/+page.ts|+page.ts]]
	- `params.metadata` contain the [[Frontmatter]]
	- `params.default` is the Render function, ie. the file contents
20. Add [[Open Graph Protocol]] tags.
	- [ ] Add slug/categories to Blog ➕ 2023-06-28 
21. Install [[Shiki]] for syntax highlighting. It was chosen because it is simpler than [[Prism]].


---

### References

<iframe width=750 height=800 src="https://joyofcode.xyz/sveltekit-markdown-blog"></iframe>


[Build a Blazing Fast SvelteKit Markdown Blog - YouTube](https://youtu.be/RhScu3uqGd0)

[[OpenProps]]
[[Lucide]]

[[mdsvex]]