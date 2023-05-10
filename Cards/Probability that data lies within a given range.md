up:: [[Probability and Statistics]]
tags:: #note/reference  #on/Probability 
X:: [[Introduction to Data Science]]

## Probability that data lies within a given range

In [[R]]:

```
mean(x <= a) - mean(x <= b)
```

Using the normal distribution:

```
pnorm(a, mean(x), sd(x)) - pnorm(b, mean(x), sd(x))
```

---
### References

