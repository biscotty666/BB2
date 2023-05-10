up:: [[Probability and Statistics]]
tags:: #source/book #note/reference
X:: [[R]] [[Introduction to Data Science]] [[Monte Carlo Simulation]]


## How many simulations do we need?

This apparently can be quite different. One way is to check for the __stability__ of the estimate.

```
B <- 10^seq(1, 5, len = 100)
compute_prob <- function(B, n = 25) {
  same_day <- replicate(B, same_birthday(n))
  mean(same_day)
}
prob <- sapply(B, compute_prob)
data.frame(logB = log10(B), prob) |>
  ggplot(aes(logB, prob)) +
  xlab("log10(B)") +
  geom_line()
```

![[Pasted image 20230419164234.png]]

This shows that it starts to stabilize at around 1000.