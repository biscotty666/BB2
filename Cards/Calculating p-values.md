up:: [[Probability and Statistics]]
tags:: #on/PnS #source/video #source/StatQuest #note/reference 
related:: [[p-value]]

## Calculating p-values

### Discrete values

The p-value consists of 3 parts:
1. The probability that random chance would result in the observation, plus
2. The probability that something _equally rare_ could be observed, plus
3. The probability that something more rare would be observed.

#### Example

If you flip a coin 5 times and get 4 heads and 1 tails, would that mean that there was something special about the coin? The _p-value_ is used to test this hypothesis.

In fact, we test the _null hypothesis_: a result of 4 heads and 1 tails mean that the coin is normal. A small p-value (<0.05) means that the hypothesis would be rejected and the coin is not normal.

The possible outcomes of 5 flips are:

| H   | T   | Perms |
| --- | --- | ----- |
| 5   | 0   | 1     |
| 0   | 5   | 1     |
| 4   | 1   | 5     |
| 1   | 4   | 5     |
| 2   | 3   | 10    |
| 3   | 2   | 10    |
|     |    Total |    35   |

The p-value would be:

$$
p=5/35 + 5/35 + 1/35 + 1/35=0.375
$$

This is not low enough to reject the null hypothesis.

### Distributions


#### Example

![[Pasted image 20230429085316.png]]

The p-value consists of 3 parts:
1. The probability (area under the blue curve) that the person is 142 cm or _more extreme_, plus
2. The probability that something else _equally or more extreme_ could be observed (ie. >=169 cm).

$$
p = 0.025 + 0.025=0.05
$$
This suggests that a measurement of 142 cm might come from another distribution since a value of 0.05 is the borderline.

If we measure someone at 141 cm, then $p=0.016+0.016=0.03$ which means we can reject the null hypothesis and that a different distribution of heights makes more sense.

---
### References

[How to calculate p-values - YouTube](https://www.youtube.com/watch?v=JQc3yx0-Q9E)
[Statistics Fundamentals - YouTube](https://www.youtube.com/playlist?list=PLblh5JKOoLUK0FLuzwntyYI10UQFUhsY9)