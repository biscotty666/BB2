---
up: [[Brian Carey]]
tags: map, log/journal
---
## Logs

### Current

> [!journal]- Study Log
>```dataview
>table without id
>file.name as Date, std-log as Notes, std-cat as Category
>where std-log
>sort file.name desc
>```

> [!journal]- Work Log
>```dataview
>table without id
>file.name as Date, wrk-log as Notes, wrk-cat as Category
>where wrk-log
>sort file.name desc
>```

> [! journal]- Exercise Log
> ```dataview
> table without id
> file.name as Date, exc-type as Type, exc-time as Time, exc-dist as Dist, exc-route as Route, exc-note as Note
> where exc-type
> sort file.name desc
> ```

> [!journal]- Domestic Log
> ```dataview
> table without id
> file.name as Date, dom-log as Notes
> where dom-log
> sort file.name desc
> ```

> [!journal]- Piano Log
> ```dataview
> table without id
> file.name as Date, pia-songs as Songs, pia-time as Time, pia-note as Notes
> where pia-time
> sort file.name desc
> ```

> [!journal]- Systems Log
> ```dataview
> table without id
> file.name as Date, laptop-log as Log, laptop-comments as Notes
> where laptop-log
> sort file.name desc
> ```


### Legacy

> [! journal]- Exercise Log - Legacy
> ```dataview
> table without id
> file.name as Date, exercise-type as Type, exercise-time as Time, exercise-dist as Dist, exercise-route as Route, exercise-note as Note
> where exercise-note
> sort file.name desc
> ```

> [!journal]- Piano Log - Legacy
> ```dataview
> table without id
> file.name as Date, piano-log as Notes, piano-time as Time
> where piano-log
> sort file.name desc
> ```

> [!journal]- Study Log - Legacy
>```dataview
>table without id
>file.name as Date, study-log as Notes
>where study-log
>sort file.name desc
>```

