up:: [[Probability and Statistics]]
tags:: #source/book #note/reference #on/Probability 
X::  [[Introduction to Data Science]]

## The Birthday Problem

### Monte Carlo

In a room with 50 people, what is the chance that at least two people have the same birthday? This is another example, like the [[Monty Hall Problem]], where intuition is misleading. We can use a  [[Monte Carlo Simulation]] in [[R]]

```
n <- 50
B <- 10000

same_birthday <- function(n) {
  bdays <- sample(1:365, n, replace = TRUE)
  any(duplicated(bdays))
}

results <- replicate(B, same_birthday(50))
mean(results)

## [1] 0.971
```

So in a room of 50 people there is a __97%__ chance that two people have the same birthday.

Now run a Monte Carlo Simulation:

First create a function which `replicate`s the above function `B` times for a group size `n`.
```
compute_prob <- function(n, B = 10000) {
  results <- replicate(B, same_birthday(n))
  mean(results)
}

n <- seq(1, 60)
prob <- sapply(n, compute_prob)

data.frame(n = n, prob = prob) |>
  ggplot(aes(n, prob)) +
  geom_point()
```

![[Pasted image 20230419163232.png]]


### Exact method

Mathematically it is simpler to calculate the probability of something not happening than something happening. We will need the [[Multiplication Rule]].

Let’s start with the first person. The probability that person 1 has a unique birthday is 1. The probability that person 2 has a unique birthday, given that person 1 already took one, is 364/365. Then, given that the first two people have unique birthdays, person 3 is left with 363 days to choose from. We continue this way and find the chances of all 50 people having a unique birthday is:

$$
1 \times \frac{364}{365}\times\frac{363}{365} \dots \frac{365-n + 1}{365}
$$

```
exact_prob <- function(n) {
  prob_unique <- seq(365, 365 - n + 1) / 365
  1 - prod(prob_unique)
}
eprob <- sapply(n, exact_prob)
data.frame(n = n, eprob = eprob) |>
  ggplot(aes(n, eprob)) +
  geom_line(col = "red") +
  geom_point(aes(n, prob))
```

![[Pasted image 20230419163814.png]]


---
### References

[13 Probabilities](https://biscotty666.github.io/Data-Science-R-PH125x/docs/Pt13.html#birthday-problem)