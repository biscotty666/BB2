---
up: [[Logs]]
type: log
related: [[Health]]
---
X:: 

# Cooking Log

```dataview
table without id
file.name as Date, cooking-log as Notes
from #log/journal 
where cooking-log
sort file.name desc
```






