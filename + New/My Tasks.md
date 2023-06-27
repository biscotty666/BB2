## Task Management



### Todays progress
```dataview
task
where completion = date(today)
```

### This week
```dataview
task
where completion >= date(today) - dur(1 week)
```
### This month
```dataview
task
where completion >= date(today) - dur(1 month)
```

### Overdue
```todoist
name: ""
filter: overdue
sorting: 
 - priority
 - date
group: true
```

### Due today
```todoist
name: ""
filter: today
sorting: 
 - priority
 - date
group: true
```

### Priority 1
```todoist
name: ""
filter: p1
sorting: 
 - date
group: true
```

### Priority 2
```todoist
name: ""
filter: p2
sorting: 
 - date
group: true
```

### Others
```todoist
name: ""
filter: p3 | p4
sorting: 
 - date
group: true
```


