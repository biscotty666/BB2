up::  [[Probability and Statistics]]
tags:: #source/book #note/reference #on/Probability 
X:: [[Introduction to Data Science]]

## Random variables

Numerical outcomes resulting from a random process. In [[R]] we can use `sample` to generate random variables:

```
beads <- rep(c("red", "blue"), times = c(2,3))
X <- ifelse(sample(beads, 1) == "blue", 1, 0)
```

`X` is a random variable. By convention capital letters are used for random variables and lower case letters for observed values.

[[Sampling models - Casino roulette winnings]]
[[Probability distribution of random variables]]



---

### Reference


