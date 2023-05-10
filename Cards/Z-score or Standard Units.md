up:: [[Probability and Statistics]]
tags:: #note/reference  #on/Statistics 
X:: [[Introduction to Data Science]]

## Z-score or Standard Units

The number of standard deviations an observation is from the mean.

$$Z=\frac{x-\mu}{\sigma}$$

In [[R]] use the `scale()` function.

Useful for seeing if an observation is about average $(z=0)$, one of the largest $(z\approx 2)$, one of the smallest $(z\approx −2)$, or an extremely rare occurrence $(z\gt 3)$ or $(z\lt −3)$. Remember that it does not matter what the original units are, these rules apply to any data that is approximately normal.

How many observations are within 2 SDs from the average?

```
z <- scale(x)
mean(abs(z) < 2)
## [1] 0.95
```

Also used to create [[Quantile-quantile plots]].

---
### References

