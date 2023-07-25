## app.html

```
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="utf-8" />
		<link rel="icon" href="%sveltekit.assets%/favicon.png" />
		<link rel="icon" href="https://fav.farm/🔥" />
		<meta name="viewport" content="width=device-width" />
		%sveltekit.head%
		<script type="module">
			const theme = localStorage.getItem('color-scheme')

			theme
				? document.documentElement.setAttribute('color-scheme', theme)
				: localStorage.setItem('color-scheme', 'dark')
		</script>
	</head>
	<body data-sveltekit-preload-data="hover">
		<div style="display: contents">%sveltekit.body%</div>
	</body>
</html>
```

Add a link for the rss feed.

```
<!DOCTYPE html>
<html lang="en">
	<head>
		<meta charset="utf-8" />
		<link rel="icon" href="%sveltekit.assets%/favicon.png" />
		<link rel="icon" href="https://fav.farm/🔥" />
		<link rel="alternate" type="application/atom+html" href="/rss.xml" />
		<meta name="viewport" content="width=device-width" />
		%sveltekit.head%
		<script type="module">
			const theme = localStorage.getItem('color-scheme')

			theme
				? document.documentElement.setAttribute('color-scheme', theme)
				: localStorage.setItem('color-scheme', 'dark')
		</script>
	</head>
	<body data-sveltekit-preload-data="hover">
		<div style="display: contents">%sveltekit.body%</div>
	</body>
</html>
```

---
up:: [[Svelte markdown blog]]
tags:: #note/example #effort/blog #source/video #on/Svelte
Source:: [[Joy of Code]]

