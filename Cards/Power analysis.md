up:: [[Probability and Statistics]]
tags:: #on/PnS #source/video #source/StatQuest #note/reference 

## Power analysis

Power refers to the probability that you can reject the null hypothesis. You have high power when comparing populations which are very different (little overlap) but low power when comparing two populations which are actually different but the difference is small (large overlap).

Power can be increased by increasing sample size. In order to determine a good sample size you can use a power analysis.

One approach:
1. Choose the desired power. Typically 0.8 is used.
2. Choose the threshold for significance $\alpha$. Typically 0.05 is used.
3. Determine the overlap. The overlap is a combination of the means and standard deviations of the populations. These are combined in a single variable, the Effect Size $d$.
$$
\text{Effect Size}(d)=\frac{\text{Estimated difference in means}}{\text{Pooled estimated standard deviations}}
$$
Where
$$
\text{Pooled estimated standard deviations}=\sqrt \frac{sd_1+sd_2}{2}
$$

An on-line calculator for this method is at [Pooled Standard Deviation Calculator - Statology](https://www.statology.org/pooled-standard-deviation-calculator/).

---

### References

[Statistical Power, Clearly Explained!!! - YouTube](https://www.youtube.com/watch?v=Rsc5znwR5FA)
[Statistics Fundamentals - YouTube](https://www.youtube.com/playlist?list=PLblh5JKOoLUK0FLuzwntyYI10UQFUhsY9)