up::  [[Probability and Statistics]]
tags:: #source/book #note/reference #on/Probability 
X:: 

## Calculated SD vs sd function

The standard deviation of a population is calculated. This is necessary if the _actual_ standard deviation of the data is needed:

$$
\mu = \frac{1}{n} \sum_{i=1}^n x_i
$$

$$
\sigma =  \sqrt{\frac{1}{n} \sum_{i=1}^n (x_i - \mu)^2}
$$

So in [[R]] we can do

```
library(dslabs)
x <- heights$height
m <- mean(x)
s <- sqrt(mean((x-m)^2))
m;s

## [1] 68.3
## [1] 4.08
```

The `sd()` function uses a slightly different function which estimates the standard deviation based on random samples. So instead of observations, $x$, we use samples, $X$. The actual mean, $\mu$, is substituted by the sample mean, $\hat{X}$. The sum is divided by $N-1$ instead of $n$. For large values of $N$ the difference is negligible so it can be used instead of the actual calcuculation above if $N$ is large.

$$
\bar{X} = \frac{1}{N} \sum_{i=1}^N X_i
$$

$$
s =  \sqrt{\frac{1}{N-1} \sum_{i=1}^N (X_i - \bar{X})^2}
$$



---

### References

[[Introduction to Data Science]]



