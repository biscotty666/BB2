dates:: {{date}}
tags:: #log/journal #note/daily 

---
## <% tp.file.title %>

### Quote

<% tp.web.daily_quote() %>


### Reflections

#### Thoughts

#### Progress

### Tasks

#### New today

#### Due today

```todoist
name: "Due Today"
filter: "<% tp.date.now("MMM Do YYYY") %>"
sorting: 
  - date
  - priority
group: true
```

#### Completed today

```dataview
task
where completion = date("<% tp.date.now("YYYY-MM-DD") %>")
```


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

#### Domestic Log

Dom Log:: 

#### Laptop Log

Laptop Log:: 
Laptop Comments::


---
### Yesterday's Note

<%*
const yesterday = tp.date.yesterday("YYYY-MM-DD")
tR += "![[" + [[yesterday]] + "]]"
%>


