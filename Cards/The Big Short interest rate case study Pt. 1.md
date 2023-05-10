up::  [[Probability and Statistics]]
tags:: #source/book #note/reference #on/Probability 
X:: 

## The Big Short interest rate case study Pt. 1

Assume a __2% default rate__ at a cost of __$200,000__ each. 

If your bank gives out __1,000 loans__ at __$180,000__ each:

### Estimating loss

The estimated loss would be a [[Random variables]] approximating the loss:

```
n <- 1000
loss_per_foreclosure <- -200000
p <- 0.02
defaults <- sample(c(0,1), n, prob = c(1-p, p), replace = TRUE)
sum(defaults * loss_per_foreclosure)

## [1] -4200000
```

Since this is a random variable we expect a [[Normal Distribution]], we can use the [[Central Limit Theorem]] and calculate

```
n * (p * loss_per_foreclosure + (1-p) * 0)

## [1] -4e+06

sqrt(n) * abs(loss_per_foreclosure) * sqrt(p * (1-p))

## [1] 885438
```


Confirm with a [[Monte Carlo Simulation]].

```
B <- 10000
losses <- replicate(B,{
  defaults <- sample(c(0,1), n, prob = c(1-p, p), replace = TRUE)
  sum(defaults * loss_per_foreclosure)
})
mean(losses) / 10^6

## [1] -4
```

```
library(tidyverse)
data.frame(losses_in_millions = losses/10^6) |> 
  ggplot(aes(losses_in_millions)) + 
  geom_histogram(binwidth = 0.4, col="black")
```


![[Pasted image 20230423123450.png]]


### Determining interest rate

#### Break even

Set the expected loss ($lp$) equal to the expected gain from an interest rate of $x$.
$$
lp  + x(1-p) = 0
$$
or
```
- loss_per_foreclosure*p/(1-p)

## [1] 4082
```

Dividing by the amount of the loan gives us the break-even rate:

```
options(digits = 3)
- p*(loss_per_foreclosure/(1-p)) / 180000

## [1] 0.0227
```

But there is still a 50% chance of losing money.

#### Reduce chances of losing money

What interest rate do we need to reduce the chances of losing money to 1%?

So, for the sum $S$:
$$
\mbox{Pr}(S \lt 0) = 0.01
$$
Since $S$ is approximately normal, the _expected value_ is:
$$
\mbox{E}[S] = ( lp + x(1-p))n
$$
Where the number of draws $n$ is the number of loans.

The standard error is:
$$
\mbox{SD}[S] = |x-l| \sqrt{np(1-p)}
$$

By subtracting $\mbox{E}[S]$ from each side of the inequality and dividing by $\mbox{SD}[S]$ we get:
$$
\mbox{Pr}\left(\frac{S - \mbox{E}[S]}{\mbox{SE}[S]} < \frac{ - \mbox{E}[S]}{\mbox{SE}[S]}\right)
$$
or
$$
\mbox{Pr}\left(Z <  \frac{- ( lp + x(1-p))n}{(x-l) \sqrt{np(1-p)}}\right) = 0.01
$$
$Z$ is normal random with expected value 0 and standard error 1. So we want the right side of the inequality, $z$, to be equal to `qnorm(0.01)`, which gives `-2.33`. 

$$
\mbox{Pr}(Z \leq z) = 0.01
$$

Doing the math:

$$
x = - l \frac{ np  - z \sqrt{np(1-p)}}{n(1-p) + z \sqrt{np(1-p)}}
$$
```
l <- loss_per_foreclosure
z <- qnorm(0.01)
x <- -l*( n*p - z*sqrt(n*p*(1-p)))/ ( n*(1-p) + z*sqrt(n*p*(1-p)))
x / 180000

# [1] 0.0347
```

At this rate we have an expected profit per loan of

```
(loss_per_foreclosure * p + x * (1-p))

## [1] 2124
```

Use a Monte Carlo simulation to verify:

```
B <- 10000
profit <- replicate(B, {
  draws <- sample(c(x, loss_per_foreclosure),
                  n,
                  prob = c(1-p, p),
                  replace = TRUE)
  sum(draws)
})
mean(profit)

## [1] 2119062

mean(profit < 0)

## [1] 0.0129
```

Continued in [[The Big Short Pt 2]]

---

### References

[[Introduction to Data Science]]



