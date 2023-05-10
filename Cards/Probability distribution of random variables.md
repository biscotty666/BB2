up::  [[Random variables]]
tags:: #source/book #note/reference #on/Probability 
X:: 

## Probability distribution of random variables



In [[Sampling models - Casino roulette winnings]] we created a model to calculate the payout based on repeated random sampling. What we want to do now is calculate the probability of a result falling within a certain range. For example, what is the probability that the casino loses money?

The [[Cumulative Distribution Function (CDF)]] is

$$
F(a) = \mbox{Pr}(S\leq a)
$$
And so we can calculate the probability if $a=0$ using a [[Monte Carlo Simulation]]. In this case `mean(S)` is the __expected value__ and `sd(S)` is the [[Expected Value and Standard Error]].

```
n <- 1000
B <- 10000
roulette_winnings <- function(n){
  X <- sample(c(-1,1), n, replace = TRUE, prob=c(9/19, 10/19))
  sum(X)
}
S <- replicate(B, roulette_winnings(n))

mean(S < 0)

## [1] 0.045
```

We can now [[Visualize the probability density of a random variable]].



---

### References

[[Introduction to Data Science]]
[Constructing a probability distribution for random variable | Khan Academy - YouTube](https://www.youtube.com/watch?v=cqK3uRoPtk0&list=PLSQl0a2vh4HCLjAoORNwUJLWi5ovMjba