up:: [[Probability and Statistics]] 
tags:: #source/book #note/reference #on/Probability 
X::  [[Introduction to Data Science]]

## Let's Make a Deal

This addresses the seemingly strange fact that, in the old Let's make a deal, after Monte Hall reveals a door, the contestant should always switch their choice. In fact, the odds of "winning" are 2/1 with the switch strategy.

An example of the [[Monte Carlo Simulation]] in [[R]]

```
B <- 10000

monty_hall <- function(strategy){
  doors <- as.character(1:3)
  chosen_door <- sample(c("car", "goat", "goat"))
  my_pick  <- sample(doors, 1)
  show <- sample(doors[!doors %in% c(my_pick, prize_door)],1)
  switch <- doors[!doors%in%c(my_pick, show)]
  choice <- ifelse(strategy == "stick", my_pick, switch)
  choice == prize_door
}

stick <- replicate(B, monty_hall("stick"))
mean(stick)

switch <- replicate(B, monty_hall("switch"))
mean(switch)
```
