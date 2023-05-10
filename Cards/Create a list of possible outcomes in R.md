up:: [[R]]
tags:: #note/reference #on/R #on/Probability 
X:: [[Probability and Statistics]]

## Create a list of possible outcomes in R

```
n <- 6
outcomes <- c(0,1)
l <- rep(list(outcomes), n)
possibilities <- expand.grid(l)
```

`expand.grid()`

---
### References

