up:: [[Probability and Statistics]]
tags:: #note/reference #on/Statistics 
X:: [[Introduction to Data Science]] 

## Quantiles, Quartiles, Percentiles

Quantiles are cut-off points at which all data are at or below the given level. The cut-offs take a value between 0 and 1. In [[R]], the `quantile(data, q)`  will give the `q`th quantile. 

Percentiles are the quantiles 0.01, 0.02, 0.03...0.99. Since `quantile()` accepts an array, we find them in [[R]] with:
```
p <- seq(0.01, 0.99, 0.01)
quantile(data, p)
```

Quartiles are 0.25, 0.5 and 0.75. (Confusingly there are only 3.) These are best visualized using a [[Box and Whiskers Plot]].

`summary()` can be used to get the min, quartiles and max values.

---
### References

