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
tags:: #note/example #effort/blog #source/video  #on/Svelte
Source:: [[Joy of Code]]

