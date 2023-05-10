---
up: [[Logs]]
type: log
related: [[Health]]
---
X:: 

# Work Log

```dataview
table without id
file.name as Date, wrk-log as Notes, wrk-cat as Category
from #log/journal 
where wrk-log
sort file.name desc
```






