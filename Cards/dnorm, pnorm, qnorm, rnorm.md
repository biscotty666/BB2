up:: [[R]]
tags:: #note/reference #on/R 
X:: [[Introduction to Data Science]]

## dnorm, pnorm, qnorm, rnorm

##### `rnorm(n)` 

- returns random `n` values normally distributed

##### `dnorm(x, mean = 0, sd = 1)` 

- returns the density of a vector of quantiles `x`

##### `pnorm(p, mean = 0, sd = 1)` 

- returns the distribution function of a vector of quantiles `p`. 
-  uses the symbol $\Phi$ , eg. $\Phi(-1.96) = 0.025$ or $\Phi(1.96) = 0.975$.
- returns the proportion of standard normal distributed data that are small than `p`

##### `qnorm(q, mean = 0, sd = 1)` 

- returns the quantile function 

NB `qnorm()` and `pnorm()` are inverse functions.

---
### References

