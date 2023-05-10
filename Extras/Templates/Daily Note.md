dates:: {{date}}
tags:: #log/journal #note/daily 

---
## <% tp.file.title %>

### Quote

<% tp.web.daily_quote() %>


### Progress

Work towards something

### Reflections

Any thoughts or insights

### Today


### Logs

#### Study Log
Std Log:: 
Std Cat:: 

#### Work Log
Wrk Log:: 
Wrk Cat:: 

#### Piano Log

Pia Time:: 
Pia Note:: 
Pia Songs:: 

#### Exercise Log

Exc Type:: 
Exc Route:: 
Exc Time:: 
Exc Dist:: 
Exc Note:: 

#### Domestic Log

Dom Log:: 

#### Laptop Log

Laptop Log:: 
Laptop Comments::

### Tasks

#### Overdue

```tasks
not done
due before today
```


#### Due today

```tasks
not done
due today
status.type is not IN_PROGRESS
```

#### Completed Today

```dataview
task
where completion = date(today)
```


#### High and medium priority

```tasks
not done
(priority is high) OR (priority is medium)
sort by priority
```

#### In Progress

```tasks
status.type is IN_PROGRESS
```

#### Others

```tasks
not done
(priority is not high) AND (priority is not medium) AND (is not recurring)
sort by priority
```


---
## Yesterday's Note

<%*
const yesterday = tp.date.yesterday("YYYY-MM-DD")
tR += "[[" + [[yesterday]] + "]]"
%>


