up:: [[R]]
tags:: #note/reference #on/R 
X:: [[Probability and Statistics]]

## Determine the value at a certain percentile

Use `qnorm()`

Example:

```
male_avg <- 69
male_sd <- 3

# Determine the height of a man in the 99th percentile of the distribution.

qnorm(.99, male_avg, male_sd)
```

---
### References

