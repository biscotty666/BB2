up:: [[R]]
tags:: #note/reference  #on/R 
X:: 

## Reorder gotcha in R

Using `data |> mutate(reorder(state, murder_rate)` does not work. Instead use `mutate(state = reorder(state, murder_rate))`.

---
### References

