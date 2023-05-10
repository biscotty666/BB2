# Study Log

```dataview
table without id
file.name as Date, std-log as Notes, std-cat as Category
from #log/journal 
where std-log
sort file.name desc
```


```dataview
table without id
file.name as Date, study-log as Notes
from #log/journal 
where study-log
sort file.name desc
```

---

up:: [[Logs]]
tags:: #log  






