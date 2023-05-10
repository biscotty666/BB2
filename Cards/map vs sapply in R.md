up:: [[Introduction to Data Science]]
tags:: #source/book #note/reference #on/R 
X::

## map vs sapply in R

In [[R]] `map()` is from the __purr__ package. It uses the same syntax as `sapply()` but is strictly typed and will return errors in case of mismatch. Specific data types can be requested, eg. `map_dbl` to ensure numeric data types.