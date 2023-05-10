up:: [[Probability and Statistics]]
tags:: #source/book #note/reference #on/Statistics 
X::  [[Introduction to Data Science]]

## Gamma distribution

A continuous probability distribution used to model the time that elapses before $\alpha$ occurrences of a randomly occurring event.

Shape $\alpha$ and Scale $\theta$ . Sometimes the rate parameter $\lambda=1/\theta$ is used instead of $\theta$. Since $\theta$ is the average time between occurrences, $\lambda$ is the number of occurrences per unit time.

### Cumulative distribution function

Let $X$ equal the elapsed time before the $\alpha^\text{th}$ event and $\theta$ be the average time between occurrences. The [[Cumulative Distribution Function (CDF)]] of $X$ is

$$
F(x) = P(X\le x) = 1 - P(\text{fewer than }\alpha \text{ occurrences before time }x)
$$
__Because we are looking at a certain number of occurrences in a fixed amount of time this is a [[Poisson distribution]].
$$
\mbox{F}(x)=1-\sum_{k=0}^{\alpha -1}\frac{x^ke^{-x / \theta}}{\theta k!}, \; x \ge0
$$

The expression being summed is the probability of exactly $k$ occurrences in time $x$ in the Poisson distribution with scale $\theta$.

### Probability density function

Differentiating and simplifying gives the Probability Density Function (pdf) of $X$.
$$
f(x) = \frac{x^{\alpha - 1}e^{-x/\theta}}{(\alpha - 1)!\theta^\alpha}
$$

To allow for non-integer values of $\alpha$ we use the __gamma function__ $\Gamma(n)=(n-1)!$  where $n$ is an integer.
$$
f(x) = \frac{x^{\alpha - 1}e^{-x/\theta}}{\Gamma(\alpha) \theta^\alpha}
$$
Using $\lambda=1/\theta$:
$$
f(x) = \frac{x^{\alpha - 1}e^{-\lambda x}\lambda^\alpha}{\Gamma(\alpha)}
$$
or, alternatively:
$$
\mbox{P}(y\,|\, \alpha,\lambda)=\frac{\lambda^\alpha}{\Gamma(\alpha)}y^{\alpha-1}e^{-\lambda y}
$$


### Expected value, variance

$$
\mu=\alpha\theta
$$
$$
\sigma^2=\alpha\theta^2
$$

### Special cases

1. When $\alpha=1$, we have an [[Poisson vs exponential distribution|exponential distribution]] with a PDF:
$$
f(x)=e^{-\lambda x}
$$


2. When $\theta=2$ and $\alpha=r/2$ we have a [[Chi-squared Distribution]] with $r$ degrees of freedom. This models the distribution of teh sum of the squares of $r$ random variables, each standard normal. The PDF is:
$$
f(x)=\frac{x^{r/2-1}e^{-x/2}}{\Gamma(r/2)2^{r/2}}
$$

### Examples

[[Bakery Customers]]



![[Pasted image 20230419210956.png]]

![[Pasted image 20230419211006.png]]