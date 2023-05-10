up:: [[Probability and Statistics]]
tags:: #on/PnS #source/video #source/StatQuest #note/reference 

## False Discovery Rates

False Discovery Rates are a tool to weed out bad data that looks good by controlling the number of false positives. The term is used interchangeably with the _Benjamini-Hochberg Method_.

Normally false positives are rare. When comparing samples from a population, 5% of the time you will get a p-value < 0.05, a false positive.

Mouse cells have at least 10,000 transcribed genes. If we took two samples from the same type of mice and compared all 10,000 genes 500 would result in false positives.

The _Benjamini-Hochberg method_ adjusts p-values (increases them) in a way that limits the number of false positives reported as "significant".

Begin by arranging samples by p-value. The adjusted p-value for the largest p-value is the same. Other p-values are the smaller of the previous adjusted p-value or 

$$\text{current p-value} \times \frac{\text{total number of p-values}}{\text{p-value rank}}$$  
| p-value | rank | adjusted p-value | calculation                                  |
| ------- | ---- | ---------------- | -------------------------------------------- |
| 0.91    | 10   | 0.91             | same                                         |
| 0.81    | 9    | 0.90             | 0.81 x 10 / 9 = 0.90 which is less than 0.91 |
| 0.71    | 8    | 0.89             | 0.9 x 10 / 8 =  0.89                         |
| 0.61    | 7    | 0.87             |                                              |
| 0.51    | 6    | 0.85             |                                              |
| 0.41    | 5    | 0.82             |                                              |
| 0.31    | 4    | 0.77             |                                              |
| 0.21    | 3    | 0.70             |                                              |
| 0.11    | 2    | 0.55             |                                              |
| 0.01    | 1    | 0.1              |                                              |

The p-values which were statistically significant do not have adjusted p-values that are significantly different.


![[Pasted image 20230429221648.png]]

In an experiment we test all active genes in neuronal cells and compare one group treated by a drug to a control group.

The drug might affect 1,000 genes. Since the samples are from two distributions we expect p-values to be skewed closer to 0. The remaining 9,000 active genes may not be affected by the drug, and so match the distribution of the control group.

![[Pasted image 20230429224515.png]]

![[Pasted image 20230429224635.png]]

![[Pasted image 20230429224803.png]]

![[Pasted image 20230429224836.png]]

![[Pasted image 20230429224913.png]]

![[Pasted image 20230429225002.png]]


---

### References

[False Discovery Rates, FDR, clearly explained - YouTube](https://www.youtube.com/watch?v=K8LQSvtjcEo)
[Statistics Fundamentals - YouTube](https://www.youtube.com/playlist?list=PLblh5JKOoLUK0FLuzwntyYI10UQFUhsY9)