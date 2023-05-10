up:: [[Probability and Statistics]]
tags:: #source/book #note/reference #on/Statistics 
X:: [[Case study self-reported student heights]] [[Introduction to Data Science]]

## Outliers

Data points which differ significantly from other observations.

Could be due to:
- Faulty equipment/missing measurements
- Human error
- An actual outlier

Outliers can have a significant impact on the [[Mean]].

The [[Median]] and the [[Inter Quartile Range (IQR)]] on the other hand is robust to outliers.

The [[Median Absolute Deviation]] can be used to robustly estimate the [[Standard Deviation]]. [[R]] provides the `mad` function for this.

Outliers can be seen clearly on a [[Box and Whiskers Plot]].

__John Tukey__ defined outliers based on the top whisker of the Box Plot which is $Q_3 + 1.5\times IRQ$ and the bottom which is is $Q_1 - 1.5 \times IRQ$. An outlier is defined therefore as anything outside of the range:

$$
[Q_1 - 1.5 \times (Q_3-Q_1),Q_3 + 1.5 \times (Q_3-Q_1)]
$$

By using 3 instead of 1.5 we can identify _extreme outliers_.  Note that `geom_boxplot` takes an argument `outlier.size` which defaults to 1.5.

---

### References

[12 Summary Statistics](https://biscotty666.github.io/Data-Science-R-PH125x/docs/Pt12.html#outliers)

[Outlier - Wikipedia](https://en.wikipedia.org/wiki/Outlier)