dates:: {{date}}
tags:: #log/journal #note/daily 

---
## <% tp.file.title %>

### Quote

<% tp.web.daily_quote() %>


### Reflections

#### Thoughts

#### Progress

### Today


### Tasks


> [!warning]+
>```todoist
>name: "Due Today"
>filter: "<% tp.date.now("MMM Do YYYY") %>"
>sorting: 
>  - date
>  - priority
>group: true


> [!success]-
>```todoist
>name: "Created Today"
>filter: "created:<% tp.date.now("MMM Do YYYY") %>"
>sorting: 
>  - date
>  - priority
>group: true


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


---
## Yesterday's Note

<%*
const yesterday = tp.date.yesterday("YYYY-MM-DD")
tR += "[[" + [[yesterday]] + "]]"
%>


