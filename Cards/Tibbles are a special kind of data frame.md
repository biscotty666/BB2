up:: [[Introduction to Data Science]]
tags:: #source/book #note/reference #on/R 
X:: [[R]] 

## Tibbles are a special kind of data frame

In [[R]] a tibble, `tbl` is a data frame of a special type. Data returned by `group_by()` and `summarize()` are tibbles.  Imported data are by default tibbles. Subsets of tibbles are tibbles.  Tibbles are preferred because:

- They can be grouped
- They display better
- They can contain Complex entries
eg. 

```
tibble(id=c(1, 2, 3), func = c(mean, median, sd))
```

In this case the function column contains the associated functions

