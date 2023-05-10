up:: [[Random variables]]
tags:: #source/book #note/reference
X::  [[R]]

## Sampling models

This is an example of a sampling model. A roulette wheel has 18 green slots, 18 red slots and 2 black slots. Bets are $1 and the casino pays out $1 for each ball in a red slot.

How much can the casino expect to win if 1,000 people play.

```
color <- rep(c("Red", "Black", "Green"), c(18, 18, 2))
n <- 1000
X <- sample(ifelse(color == "Red", -1, 1), n, replace = TRUE)
X[1:10]

##  [1] -1 -1  1  1 -1 -1  1  1  1 -1
```

`X` is an example of a Random variable. More succinctly:

```
X <- sample(c(-1,1), n, replace = TRUE, prob = c(9/19, 10/19))
X[1:10]

##  [1] -1 -1  1  1 -1 -1  1  1  1 -1
```

And the winnings are:

```
sum(X)

## [1] 28
```

Note that the answer will be different each time the code is run. So what would be more useful would be the [[Probability distribution of random variables]]. 


---

## References

[14 Random variables](https://biscotty666.github.io/Data-Science-R-PH125x/docs/Pt14.html#sampling-models)
[[Introduction to Data Science]]


