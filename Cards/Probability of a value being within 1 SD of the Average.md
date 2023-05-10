up:: [[Probability and Statistics]]
tags:: #note/reference #on/Probability 
X:: 

## Probability of a value being within 1 SD of the Average

Use the `pnorm()` function in [[R]].

Example:

```
female_avg <- 64
female_sd <- 3

taller <- pnorm((female_avg + female_sd), female_avg, female_sd)

shorter <- pnorm((female_avg - female_sd), female_avg, female_sd)

# Calculate the probability that a randomly selected female is between the desired height range.   

taller - shorter
```

---
### References

