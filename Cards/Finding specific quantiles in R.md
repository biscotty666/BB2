up:: [[Introduction to Data Science]]
tags:: #source/book #note/reference #on/R 
X:: 

## Finding specific quantiles in [[R]]

The `quantile()` function takes as input a variable (column) and an array of quantile values. So `quantile(variable, c(0, 0.5, 1)` would return the minimum, median (50% quantile), and maximum. `c(0, 0.1, 0.2, 0.3, 0.4, 0.5, 0.6, 0.7, 0.8, 0.9, 1)` is another example.