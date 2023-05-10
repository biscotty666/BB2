up:: [[Probability and Statistics]]
tags:: #source/book #note/reference #on/Probability 
X:: [[Introduction to Data Science]]

## Celtics and Cavs

The Celtics are playing the Cavs in a 7-game series with the Cavs having a 60% chance of winning each game. What is the probability that the Celtics win at least 1 game.

```
events <- replicate(B, sample(c(0,1), 
                              4, 
                              replace = TRUE, 
                              prob = c(0.6, 0.4)))
mean(events == 1)

## [1] 0.401
```

This can also be done with a [[Monte Carlo Simulation]].

```

B <- 10000
results <- replicate(B, {
	celtic_wins <- sample(c(0, 1), 4, replace = TRUE, prob = c(0.6, 0.4))
	mean(celtic_wins)
})
```

