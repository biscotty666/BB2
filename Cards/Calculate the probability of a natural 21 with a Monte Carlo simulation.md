up:: [[Probability and Statistics]]
tags:: #source/book #note/reference #on/Probability 
X:: [[R]] [[Introduction to Data Science]] [[Conditional Probabilities]]

## Calculate the probability of a natural 21 with a Monte Carlo simulation


```
aces <- paste("Ace", suits)
facecard <- c("Ten", "Jack", "Queen", "King")
facecard <- expand.grid(number = facecard, suit = suits)
facecard <- paste(facecard$number, facecard$suit)

hand <- sample(deck, 2)
```

To check if the hand is a natural 21:
```
(hands[1] %in% aces & hands[2] %in% facecard) | 
  (hands[2] %in% aces & hands[1] %in% facecard)

## [1] FALSE
```

Run the [[Monte Carlo Simulation]]:

```
blackjack <- function(){
   hand <- sample(deck, 2)
  (hand[1] %in% aces & hand[2] %in% facecard) | 
    (hand[2] %in% aces & hand[1] %in% facecard)
}

B <- 10000
results <- replicate(B, blackjack())
mean(results)

## [1] 0.0451
```



