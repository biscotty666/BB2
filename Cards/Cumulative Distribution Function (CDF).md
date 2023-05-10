up:: [[Probability and Statistics]]
tags:: #source/book #note/reference #on/Statistics 
X:: [[Introduction to Data Science]] [[dnorm, pnorm, qnorm, rnorm]]

## Cumulative Distribution Function (CDF)


Cumulative distribution functions are used to calculate the area under the curve to the left from a point of interest. It is used to calculate the _accumulated probability_.

The probability density function describes the shape of the distribution (uniform, exponential, normal).

### Uniform distributions

##### PDF
$$
F(x)=\frac{1}{b-a}
$$

##### CDF
$$
\begin{align}
A=B\cdot H&=(x-a)F(x) \\ \\
&=(x-a)\bigg(\frac{1}{b-a}\bigg)
\end{align}
$$
$$
A_L=P(X\le x)=\frac{x-a}{b-a}
$$

##### Statistics
$$
\mu=E(x)=\frac{a+b}{2}
$$
$$
\sigma=\frac{b-a}{\sqrt{12}}
$$

### Exponential distributions

Uses a maximum value (rate) parameter $\lambda$ which then decreases over time.

##### PDF
$$
F(x)=\lambda e^{-\lambda x}
$$
$$
\lambda=\frac{1}{\mu}
$$
##### CDF
$$
A_L=P(X\le x) = 1-e^{-\lambda x}
$$



$$F(a)=P(X\le a)$$

The probability that $X$ lies in the semi-closed interval $(a,b]$.

$$P(a\lt X \le b)=F_x(b)-F_x(a)$$


In [[R]]:

```
library(tidyverse)
library(dslabs)
data(heights)
x <- 
  heights %>% 
  filter(sex == "Male") %>% 
  pull(height)
F <- function(a) mean(x<=a)
```
What is the chance he is taller than 70 inches?
```
1 - F(70)

## [1] 0.377
```

`pnorm(a, m, s)` provides the cumulative distribution for a normal distribution.


---

### References

[13 Probabilities](https://biscotty666.github.io/Data-Science-R-PH125x/docs/Pt13.html#cumulative-distribution-functions)
[Cumulative distribution function - Wikipedia](https://en.wikipedia.org/wiki/Cumulative_distribution_function)