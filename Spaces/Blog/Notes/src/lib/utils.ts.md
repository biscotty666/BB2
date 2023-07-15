## utils.ts

```
type DateStyle = Intl.DateTimeFormatOptions['dateStyle']

export function formatDate(date: string, dateStyle: DateStyle = 'medium', locales = 'en') {
  const formatter = new Intl.DateTimeFormat(locales, { dateStyle })
  return formatter.format(new Date(date))
}
```

---
up:: [[Svelte markdown blog]]
tags:: #note/clearexample #effort/blog #source/video #source/JoyOfCode #on/Svelte
