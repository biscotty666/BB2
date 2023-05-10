up:: [[Probability and Statistics]]
tags:: #source/book #note/reference #on/Statistics 
X:: [[R]] [[Introduction to Data Science]]

## Distribution of IQ scores

A [[Monte Carlo Simulation]] .

```
# The variable `B` specifies the number of times we want the simulation to run.
B <- 1000

# Use the `set.seed` function to make sure your answer matches the expected result after random number generation.
set.seed(1)

# Create an object called `highestIQ` that contains the highest IQ score from each random distribution of 10,000 people.
highestIQ <- replicate(B, {
  simulated_data <- rnorm(10000, 100, 15)
  max(simulated_data)
})
```

I originally tried nesting the `rnorm` function in `max` but it didn't work 😢