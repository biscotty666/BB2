up:: [[Probability and Statistics]]
tags:: #source/book #note/reference #on/Probability 
X:: 

## Combinations of multiple choices

I got this one wrong in the class, but the answer is really simple.

A restaurant manager wants to advertise that his lunch special offers enough choices to eat different meals every day of the year. He doesn't think his current special actually allows that number of choices, but wants to change his special if needed to allow at least 365 choices.

A meal at the restaurant includes 1 entree, 2 sides, and 1 drink. He currently offers a choice of 1 entree from a list of 6 options, a choice of 2 different sides from a list of 6 options, and a choice of 1 drink from a list of 2 options.

Answer:

```
6 * nrow(combinations(6,2) * 2
```