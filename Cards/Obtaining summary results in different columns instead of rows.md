up:: [[Introduction to Data Science]]
tags:: #source/book #note/reference #on/R 
X:: 

## Obtaining summary results in different columns instead of rows

Results of `summarize()` in [[R]] returns a row for each requested summary value. To return a column for each summary value instead define a function, eg:

```
median_min_max <- function(x) {
  qs <- quantile(x, c(0.5, 0, 1))
  data.frame(median = qs[1],
             minimum = qs[2],
             maximum = qs[3])
}
heights |%3E
  filter(sex == "Female") |>
  summarise(median_min_max(height))
```

