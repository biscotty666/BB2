up:: [[Probability and Statistics]]
tags:: #source/book #note/reference #on/Statistics 
X:: [[Syntax]]  [[R]]

## Combinations and Permutations


### Combinations

Combinations don't care about order, permutations do.

In [[R]] the `expand.grid()` function gives __combinations__. [[Construct a deck of cards in R]] gives an example of calculating probabilities based on combinations.

```
expand.grid(pants = c("blue", "black"),
            shirt = c("white", "grey", "plaid"))
```


| pants | shirt |
| ----- | ----- |
| blue  | white |
| black | white |
| blue  | grey  |
| black | grey  |
| blue  | plaid |
| black | plaid |

```
combinations(n, r, v = 1:n, set = TRUE, repeats.allowed = FALSE)
```

### Permutations

`permutations(n, r)` gives the number of ways `r` elements can be selected from a group of `n`. __Order Matters__.



