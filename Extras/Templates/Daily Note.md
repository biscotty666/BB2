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


```todoist
name: ""
filter: "created: <% tp.date.now("MMM Do YYYY") %>"
```



#### Due today

```todoist
name: "Due Today"
filter: "<% tp.date.now("MMM Do YYYY") %>"
sorting: 
  - date
  - priority
group: true
```

#### Priority 1 tasks

```todoist
name: ""
filter: p1
```



### Logs

#### Study Log
Std Log:: 
Std Cat:: 

#### Piano Log

Pia Time:: 
Pia Note:: 
Pia Songs:: 

#### Domestic Log

Dom Log:: 

#### Laptop Log

Laptop Log:: 
Laptop Comments::

### Undated Tasks
```todoist
name: ""
filter: no date
group: true
```



---
### Yesterday's Note

<%*
const yesterday = tp.date.yesterday("YYYY-MM-DD")
tR += "![[" + [[yesterday]] + "]]"
%>


