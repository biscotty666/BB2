---
date: 2023-03-08
area: Back
animal: B
identification: Unknown
description: Typing what I want
comments: I should be able to enter __formatted__ text?
---

##### Date 
```meta-bind
INPUT[date_picker:date]
```
#animal/mammal
##### Area 
```meta-bind
INPUT[suggester(
option(Side),
option(Back),
option(Secret)
):area]
```
animal_type:: 1
##### Type of animal
```meta-bind
INPUT[suggester(
option(B),
option(M),
option(I),
option(S)
):animal_type]
```

##### Identification
```meta-bind
INPUT[text:identification]
```


##### Description
```meta-bind
INPUT[text_area:description]
```

##### Comments
```meta-bind
INPUT[editor:comments]
```

---
tags:: #note/observation