up:: [[Home]]
tags:: #map #to/watch #to/read

## Reading and video list

### Videos

```dataview
table without id
Priority, file.name as Topic, tags as Tags
from (#to/watch OR #videotowatch )
where file.name != "Reading and video list" and file.name != "2023-05-04"
sort Priority asc, tags
```

### Articles

```dataview
table
Priority, tags
from #to/read
where file.name != "Reading and video list"
sort Priority asc
```

---

### References