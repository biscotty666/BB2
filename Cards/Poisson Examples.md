up:: [[Poisson distribution]]
tags:: #on/Probability #source/video #note/reference 

## Poisson Examples

### Basic

For a recent period of 100 years there were 530 Atlantic hurricanes. Assuming the Poisson distribution is suitable, find the probability that 2 hurricanes will occur in a random year.

$$
\mu = \frac{\text{number of hurricanes}}{\text{number of years}}=\frac{530}{100}=5.3
$$
$$
P(2)=\frac{\mu^xe^{-\mu}}{x!}=\frac{5.3^2(2.71828)^{-5.3}}{2!}=0.0701
$$

### Different time periods

A student receives an average of 7 texts in a 2 hour period.

1. What is the probability that the student will receive exactly 9 texts in a 2 hour period?

$$
\mu=7;\; x=9
$$
$$
P(X=9)=\frac{7^9(2.71828)^{-7}}{9!}=0.1014
$$
2. What is the probability that they will receive exactly 24 in an _8 hour_ period?
$$
\mu=7\times4=28; \; x=24
$$
$$
P(24)=\frac{28^{24}e^{28}}{24!}=0.060095
$$



### Ranges

A business receives 8 calls/hour.

1. What is the probability the business will receive _at most_ 5 calls in one hour?
$$
\mu=8;
$$
$$
\begin{align}
P(x\le5)&=P(x=0)+P(1)+\dots+P(5) \\
&=e^{-8}\bigg[\frac{8^0}{0!} + \frac{8^1}{1!} + \frac{8^2}{2!} + \frac{8^3}{3!} + \frac{8^4}{4!} + \frac{8^5}{5!} \bigg] \\
&=0.1912
\end{align}
$$



2. What is the probability the business will receive _more than_ 6 calls in one hour?
$$
P(x\gt6)=1-P(x\le6)
$$
Using the earlier result:
$$
P(x\lt6)=1-0.1912-e^{-8}\frac{8^6}{6!}=0.6866
$$


---
### References

[Statistics - Binomial & Poisson Distributions - YouTube](https://www.youtube.com/watch?v=BR1nN8DW2Vg)

