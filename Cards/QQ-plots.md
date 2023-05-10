up:: [[Data Visualization]]
tags:: #note/reference #on/DataVisualization 
X:: 

## QQ-plots

Used to determine if actual data is approximated well by the standard distribution.

Plots actual values versus theoretical values.

An example with [[R]]:

```
data(heights)
index <- heights$sex=="Male"
x <- heights$height[index]
p <- seq(0.05, 0.95, 0.05)
observed_quantiles <- quantile(x, p)
theoretical_quantiles <- qnorm(p, mean = mean(x), sd = sd(x))
plot(theoretical_quantiles, observed_quantiles)
abline(0, 1)
```

---
### References

