up:: [[Probability and Statistics]]
tags:: #source/book #note/reference
X:: 

## Probability Density


For categorical distributions, we can define the probability of a category, eg. a dice roll, as

$$
Pr(X = 4) = 1/6
$$

The [[Cumulative Distribution Function (CDF)]] would be 

$$
F(4) = \mbox{Pr}(X\leq 4) =  \mbox{Pr}(X = 4) +  \mbox{Pr}(X = 3) +  \mbox{Pr}(X = 2) +  \mbox{Pr}(X = 1)
$$

We can use a __theoretical definition__ with a similar interpretation. The probability density of $x$ is defined as $f(a)$ such that

$$
F(a) = \mbox{Pr}(X\leq a) = \int_{-\infty}^a f(x)\, dx
$$
