up:: [[Probability and Statistics]]
tags:: #source/book #note/reference #on/Probability 
X:: [[Introduction to Data Science]]

## Monte Carlo Simulation for continuous variables

In [[R]] use `rnorm` to generate normally distributed data based on a mean and standard deviation.

```
library(tidyverse)
library(dslabs)
data(heights)
x <- 
  heights %>% 
  filter(sex == "Male") %>% 
  pull(height)
```

For 800 males chosen at random what is the distribution of the tallest person? How rare is a 7 foot person? We use `rnorm` with the mean and sd from the data to select people in a [[Monte Carlo Simulation]].

```
B <- 10000
tallest <- replicate(B, {
  simulated_data <- rnorm(800, m, s)
  max(simulated_data)
})
mean(tallest >= 7*12)

## [1] 0.0214
```


```
data.frame(tallest = tallest) %>% 
  ggplot(aes(tallest)) +
  geom_histogram(color = "black", binwidth = 1)
```


![[Pasted image 20230419213710.png]]


---
### References

[13 Probabilities](https://biscotty666.github.io/Data-Science-R-PH125x/docs/Pt13.html#monte-carlo-simulations-for-continuous-variables)
