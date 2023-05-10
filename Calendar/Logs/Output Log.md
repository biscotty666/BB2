---
up: [[Logs]]
type: log
related: [[Health]]
---
X:: 

# Output Log

```dataview
table without id
file.name as Date, output-log as Notes
from #log/journal 
where output-log
sort file.name desc
```






