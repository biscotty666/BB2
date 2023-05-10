up::  [[Probability and Statistics]]
tags:: #source/book #note/reference #on/Probability 
X:: 

## The Big Short Pt 2

### What about giving out more loans?

More loans implies a _higher default rate_. What if we chose a default rate of 4% and an interest rate of 5%?

```
p <- 0.04
(- loss_per_foreclosure * p/(1-p)) / 180000 

## [1] 0.0463

r <- 0.05
x <- r * 180000
(loss_per_foreclosure * p) + (x * (1-p))

## [1] 640
```

Now we can minimize our chances of losing money by giving our more loans (increase $n$). From [[The Big Short interest rate case study Pt. 1]]

$$
\mbox{Pr}(S < 0) = 
\mbox{Pr}\left(Z < - \frac{\mbox{E}[S]}{\mbox{SE}[S]}\right)
$$
$Z$ is a [[Standard normal variable Z]]. With expected value $\mu$ and standard deviation $\sigma$ and $z$ = `qnorm(0.01)`:

$$
-\frac{\mbox{E}[S]}{\mbox{SE}[S]} = - \frac{n\mu}{\sqrt{n}\sigma} = - \frac{\sqrt{n}\mu}{\sigma} = z
$$

If we let 

$$
n \geq z^2 \sigma^2 / \mu^2
$$
We are guaranteed a probability of less than 0.01. The implication is that, as long as $\mu$  is positive, we can find an $n$ that minimizes the probability of a loss. This is a form of the law of large numbers: when $n$  is large, our average earnings per loan converges to the expected earning $\mu$.

We want the probability of failure to be 1%.

```
z <- qnorm(0.01)
n <- ceiling((z^2 * (x-l)^2 * p * (1-p))/
               (l * p + x * (1-p))^2)
n

## [1] 22163
```

with an expected total of

```
n * (loss_per_foreclosure * p + x * (1-p))

## [1] 14184320
```

Confirm with a [[Monte Carlo Simulation]].

```
p <- 0.04
x <- 0.05 * 180000
profit <- replicate(B, {
  draws <- sample(c(x, loss_per_foreclosure), n,
                  prob = c(1-p,p), replace = TRUE)
  sum(draws)
})
mean(profit)

## [1] 14133512
```

### But the problem is...

We are assuming that each draw is independent. In other words that each default has no impact on subsequent bank loans. But in fact a large number of defaults affects the stability of the bank and can impact future draws. Also the economic climate (interest rates) might change over that period making our rate more or less attractive. (Maybe do a [[Time Series Analysis (TSA)]]). 

Based on 

$$
\mbox{SE}[(X_1+X_2+\dots+X_n) / n] = \sigma / \sqrt{n}
$$
we can minimize $\mbox(SE)$ by making $n$ large. But we need to account for "real world events" somehow.

We can build a random global event into _each draw_ which increases or decreases _everyone's_ rate $\pm0.01$.

```
p <- 0.04
x <- 0.05*180000
profit <- replicate(B, {
    new_p <- p + sample(seq(-0.01, 0.01, length = 100), 1)
    draws <- sample( c(x, loss_per_foreclosure), n, 
                        prob=c(1-new_p, new_p), replace = TRUE) 
    sum(draws)
})

mean(profit)
## [1] 14124943

mean(profit < 0)
## [1] 0.344

mean(profit < -10^6)
# [1] 0.334
```

While profit is healthy the chance of losing money is also high, including the chance of losing more than $10M. This was what was not considered in the financial meltdown of 2007.

```
data.frame(profit_in_millions = profit / 10^6) |>
  ggplot(aes(profit_in_millions)) +
  geom_histogram(color = "black", binwidth = 5)
```

![[Pasted image 20230423220017.png]]


---

### References

[14 Random variables](https://biscotty666.github.io/Data-Science-R-PH125x/docs/Pt14.html#the-big-short)
[[Introduction to Data Science]]



