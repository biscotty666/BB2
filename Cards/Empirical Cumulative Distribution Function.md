up::  [[Probability and Statistics]]
tags:: #source/book #note/reference #on/Statistics #on/Visualization 
X:: [[R]]

## Empirical Cumulative Distribution Function eCDF

$$F(a)=\text{Proportion of data points >=a}$$
Called __Empirical__ because the [[Cumulative Distribution Function (CDF)]] is determined mathematically without data.

Defines the distribution for numerical data. It can be plotted in [[R]] with:

```
heights |>
  filter(sex == "Male") |>
  ggplot(aes(height)) +
  stat_ecdf() +
  ylab("F(a)") + xlab("a")
```


---

### Reference

[[Introduction to Data Science]]

[12 Summary Statistics](https://biscotty666.github.io/Data-Science-R-PH125x/docs/Pt12.html#empirical-cumulative-distribution-functions)

[Empirical distribution function - Wikipedia](https://en.wikipedia.org/wiki/Empirical_distribution_function)



