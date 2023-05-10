up:: [[R]]
tags:: #note/reference #on/R #on/Syntax
X:: [[Syntax ]]

## Construct a deck of cards in R

[[R]]:

```
ranks <- c("Ace", "Two", "Three", "Four", "Five", "Six", "Seven",
           "Eight", "Nine", "Ten", "Jack", "Queen", "King")
suits <- c("Hearts", "Spades", "Diamonds", "Clubs")
deck <- expand.grid(rank = ranks, suit = suits)
deck <- paste(deck$rank, deck$suit)
```

With the deck constructed, we can double check that the probability of a King in the first card is 1/13 by computing the proportion of possible outcomes that satisfy our condition:

```
kings <- paste("King", suits)
mean(deck %in% kings)

## [1] 0.0769
```

---
### References

[13.6 Combinations and permutations](https://biscotty666.github.io/Data-Science-R-PH125x/docs/Pt13.html#combinations-and-permutations)



