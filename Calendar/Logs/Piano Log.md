# Piano Practice Log

```dataview
table without id
file.name as Date, pia-songs as Songs, pia-time as Time, pia-note as Notes
from #log/journal 
where pia-time
sort file.name desc
```

```dataview
table without id
file.name as Date, piano-log as Notes, piano-time as Time
from #log/journal 
where piano-log
sort file.name desc
```

---

up:: [[Logs]]
tags:: #on/Music #log
related:: [[Music]]






