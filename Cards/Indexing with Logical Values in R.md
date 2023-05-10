up:: [[Introduction to Data Science]]
tags:: #source/book #note/reference #on/R 
X:: 

## Indexing with Logical Values in [[R]]

Logical statements such as `rate > 4` can be used to subset a dataset based on conditionals applied to specific variables in the set.  In order to do this define an index like:

`index <- conditional` eg. `index <- murder_rate > 1` and then using the new index to subset the dataset like `dataset$other_variable[index]` which returns only `other_variable` values where the `murder_rate` variable is greater than 1.

Indices can also be constructed using `which()`, `match()` and `%in%`.

 
---
#### Reference

[2.13.1 Subsetting with logicals](https://biscotty666.github.io/Data-Science-R-PH125x/docs/Pt02.html#subsetting-with-logicals)
