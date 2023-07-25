up:: [[Open Graph Protocol]]
tags:: #note/reference #source/article 
Source:: [[freecodecamp.org]]

## Open Graph Basic Tags

- `og:title`: The title of your page. This is typically the same as your webpage's `<title>` tag unless you’d like to present it differently.
- `og:type`: The “type” of website you have. I’ll explain more in the next section, though a generic “type” is “website”.
- `og:image`: This should be a link to an image that you’d like to represent your content. It should be a high resolution image that the social networks will use in their feeds.
- `og:url`: This should be the URL of the current page.

Tags are placed in the `<head>` using `meta` tags.

```
<meta property=“[NAME]” content=“[VALUE]” />
```

eg.

```
<meta property="og:title" content="biscotty's blog" />
```

---
### Reference

[[What is Open Graph and how can I use it for my website-#Starting with the basics of open graph]]