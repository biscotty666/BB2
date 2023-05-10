up::  [[Probability and Statistics]]
tags:: #source/book #note/reference #on/R #on/Statistics 
X:: [[Syntax]] [[R]]

## Inter Quartile Range (IQR)

The IQR is the difference between the 3rd (75th) and 1st (25%) quantile.

Outliers are any values outside the range $[Q_1-1.5\cdot\text{ IQR},\,Q_3+1.5\cdot\text{IQR}]$.

### Finding outliers

1. Given the numbers: 5, 8, 15, 26, 10, 18, 3, 12, 6, 14, 11.
2. Place in ascending order: 3, 5, 6, 8, 10, 11, 12, 14, 15 ,18 ,26
3. Identify the median ($Q_2$): 11
4. Identify the median of the lower half of the data ($Q_1$): 6
5. Identify the median of the upper half of the data ($Q_3$): 15
6. In groups with an even number of elements, average the two middle numbers to determine the quartile.

$$
\text{IQR}=Q_3-Q_1=15-6=9
$$
Is 26 an outlier?

The range of not outliers would be
$$
[6-(1.5\cdot 9),\,15+(1.5\cdot 9)]=[-7.5,\,28.5]
$$
Therefore 26 is not an outlier.


The IQR is best visualized with a [[Box and Whiskers Plot]].

Like the [[Median]], the IQR is robust with respect to outliers.

[[R]] provides the `IQR()` function for this purpose.

```
IQR(x, na.rm = FALSE, type = 7)
```
