up:: [[Probability and Statistics]]
tags:: #source/book #note/reference #on/Probability  #on/R 
X:: [[R]]  [[Introduction to Data Science]]

## Monty Hall Problem

The Monty Hall Problem, like the [[The Birthday Problem]] is an example of where statistical results are non-intuitive. The question is, after a contestant picks a door and the host reveals one of the two other doors, is it better to switch doors when given the choice or stick with the originally selected door. In fact, the switch strategy is twice as good as the stick strategy. This seems odd at first, but is perfectly natural when you realize that the host never "shows" the prize door.

This can be shown with a [[Monte Carlo Simulation]].

```
doors <- as.character(1:3)
prize <- sample(c("car", "goat", "goat"))
prize_door <- doors[prize == "car"]
my_pick  <- sample(doors, 1)
```

Our `monty_hall` function 

1. create three doors and assign the car prize to one of the doors. 
2. samples the doors and assign to `my_pick`
3. use `sample` again to store the remaining door (the one not chosen AND the one with the prize) (Is there not a better way to do this?) in the variable `show`.  This will be shown if the strategy is "stick". 
4. Store the door that will be shown if the `strategy` is _stick_.
5. Store the choice, depending on the `strategy`, in `choice`.
6. Check if the choice is the prize door.

```
B <- 10000
monty_hall <- function(strategy){
  doors <- as.character(1:3)
  prize_door <- doors[prize == "car"]
  my_pick  <- sample(doors, 1)
  show <- sample(doors[!doors %in% c(my_pick, prize_door)],1)
  switch <- doors[!doors%in%c(my_pick, show)]
  choice <- ifelse(strategy == "stick", my_pick, switch)
  choice == prize_door
}
stick <- replicate(B, monty_hall("stick"))
mean(stick)

## [1] 0.335
```
```
switch <- replicate(B, monty_hall("switch"))
mean(switch)

## [1] 0.668
```




---
### References

[13 Probabilities](https://biscotty666.github.io/Data-Science-R-PH125x/docs/Pt13.html#monty-hall-problem)
