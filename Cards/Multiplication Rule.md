up:: [[Probability and Statistics]]
tags:: #source/book #note/reference #on/Probability 
X::  [[Introduction to Data Science]]

## Multiplication Rule

Used to determine the probability of multiple events occurring. This represents a logical `AND`. For a logical `OR` use the [[Addition Rule]]

For 2 events:
$$
\mbox{Pr}(A \mbox{ and } B) = \mbox{Pr}(A)\mbox{Pr}(B \mid A)
$$

For multiple events:

$$
\mbox{Pr}(A \mbox{ and } B \mbox{ and } C) = \mbox{Pr}(A)\mbox{Pr}(B \mid A)\mbox{Pr}(C \mid A \mbox{ and } B)
$$

For multiple events under independence. Data should be checked to ensure actual independence of events.

$$
\mbox{Pr}(A \mbox{ and } B \mbox{ and } C) = \mbox{Pr}(A)\mbox{Pr}(B)\mbox{Pr}(C)
$$

---
### Reference

[13 Probabilities](https://biscotty666.github.io/Data-Science-R-PH125x/docs/Pt13.html#multiplication-rule)