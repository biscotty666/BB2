up:: [[Home]]
tags:: #map/view 

# The Notebox 🗃

## Develop 🍃

```dataview
TABLE
tags as Tags
FROM #note/develop🍃 
SORT file.name asc
```

## Investigate 🔎


```dataview
TABLE
priority as Priority, tags as Tags
FROM #note/investigate🔎 
SORT priority
```

## Boats 🚤

```dataview
List
from #note/boat🚤 
sort file.name asc
```