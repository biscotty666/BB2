up:: [[Svelte]]
tags:: #source/video #effort/blog #on/Svelte #note/example
Source:: [[Joy of Code]]

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
22. Modify [[svelte.config.js]] 
23. For demonstration, create [[Spaces/Blog/Notes/src/posts/counter.svelte|counter.svelte]] 
24. Import and use the component in [[second-post.md]] 
25. Create a layout for custom components (see [mdsvex docs!](https://mdsvex.pngwn.io/docs#layout)). NB. the name of the layout is arbitrary. The presenter prefers [[Spaces/Blog/Notes/src/mdsvex.svelte|msdvex.svelte]] to `layout.svelte` to avoid confusion with svelte's own layouts.
26. Modify [[svelte.config.js]]
27. Create [[Spaces/Blog/Notes/src/lib/components/custom/img.svelte]]. NB. this must have the same name as the component you wish to customize. 
28. Also create [[Spaces/Blog/Notes/src/lib/components/custom/index.ts]] which is used to export the components. NB. According to the presenter, in this case, with one component, 2 files is "overkill".
29. Install plugins `pnpm i remark-unwrap-images remark-toc rehype-slug`. These remove `<p>` tags from around images, provides a table of contents by creating a header with the name `Table of Contents`, and provide the slug as a variable.
30. Add ` { tight: true } ` to the TOC configuration in [[svelte.config.js]] to remove extraneous `<li>` tags.
31. Load theme in [[Spaces/Blog/Notes/src/app.html]] to prevent flashing.
32. Create [[Spaces/Blog/Notes/lib/theme.ts]] as a Svelte store
33. Create [[Spaces/Blog/Notes/src/toggle.svelte]] 
34. Add the new component to [[header.svelte]] 
35. To add page transitions, create [[Spaces/Blog/Notes/src/routes/+layout.ts]] to watch for changes in the url pathname.
36. Create a new component [[Spaces/Blog/Notes/src/routes/transition.svelte]] 
37. Add the new component to [[+layout.svelte]] 
38. To create a stand-alone endpoint for an rss feed create [[Spaces/Blog/Notes/src/routes/rss.xml/+server.ts]] 
39. Add a `<link>` to [[app.html]] to allow subscribing to the feed from the base url.
40. To override the default error template create [[Spaces/Blog/Notes/src/+error.svelte]] 
41. Create About and Contact routes
42. To pre-render as a static site edit [[+layout.ts]] and [[Spaces/Blog/Notes/src/routes/rss.xml/+server.ts|+server.ts]] 
43. To deploy to Vercel, remove the default adapter and add the one for vercel: `pnpm remove @sveltejs/adapter-auto && pnpm i -D @sveltejs/adapter-vercel`.
44. Replace `adapter-auto` with `adapter-vercel` in [[svelte.config.js]]
45. `pnpm run build && pnpm run preview`
46. Push to git and deploy to vercel.


---

### References

<iframe width=750 height=800 src="https://joyofcode.xyz/sveltekit-markdown-blog"></iframe>


[Build a Blazing Fast SvelteKit Markdown Blog - YouTube](https://youtu.be/RhScu3uqGd0)

[[OpenProps]]
[[Lucide]]

[[mdsvex]]