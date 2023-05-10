up:: [[R]]
tags:: #note/reference #on/R
X:: 

## Computed variables in R

- density: density estimate
- count: density * number of points - useful for stacked density plots.
- scaled: density estimate, scaled to maximum of 1.
- n: number of points.
- ndensity: alias for scaled


Used inside a __ggplot__ `aes()`

Eg:

```
ggplot(faithful, aes(x = waiting)) +
  geom_histogram(aes(y = after_stat(density))) +
  geom_density()
```


---
### References

