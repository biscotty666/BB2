up:: [[Binomial Distribution]]
tags:: #on/PnS #source/video #note/example #note/reference  #source/OrgChem

## Problems on Continuous Uniform Distributions

### Train waiting

The amount of time a person must wait is uniformly distributed between 0 and 40 minutes.
1. What is the probability density function?
2. Graph the probability density function.
3. What is the probability the person must wait less than 8 minutes
4. What is the probability the person must wait more than 30 minutes
5. Calculate P(10<x<26), P(x=20), P(x>45)
6. Calculate the mean and standard deviation
7. What is the 85th percentile?

1. $F(x)=\frac{1}{40-0}=0.025$
2. A rectangular area from 0 to 40 on the x-axis and a height of $\frac{1}{40}$.
3. The area with a base of 8 and a height of $\frac{1}{40}$, so $\frac{8}{40}$ or $\frac{1}{5}$ or 20%.
4. The area with a base of 10 and a height of $\frac{1}{40}$, so $\frac{10}{40}$ or $\frac{1}{4}$ or 25%.
5. 40%, 0% (a line has no area), 0%
6. $\mu=\frac{a+b}{2}$ or 20, $\sigma=\frac{40-0}{\sqrt{12}}=11.547$
7. 34

### Test completion time

The amount of time it takes a student to complete a test is uniformly distributed between 20 and 45 minutes.
1. Write the probability density function
$F(x)=\frac{1}{45-20}=\frac{1}{25}=.04=4$%

2. Draw a graph
	Rectangular area y=.04, x=\[20,45\] 
3. Probability more than 36 minutes
	.04x9 = 36%
1. Between 26 and 35 minutes
	.04x9 = 36%
2. Median, variance, standard distribution
	For a uniform distribution the mean and median are the same, so
$$
Median=\frac{20+45}{2}=32.5
$$
$$
\text{Variance } \sigma^2=\frac{(b-a)^2}{12}=\frac{25^2}{12}=52.083
$$
 $$
	\sigma=\sqrt{Variance}=7.217
	$$

1. Value of the 3rd quartile?
	.75x(45-20)+20=38.75
1. Given that a student always takes more than 30 minutes, what is the probability he will take more than 40 minutes. This is a conditional probability, so
$$
\begin{align}
\mbox{Pr}(A|B)&=\frac{\mbox{Pr}(A\cap B)}{\mbox{Pr}(B)} \\ \\
\mbox{Pr}(x\gt 40\,|\,x\gt 30)&=\frac{\mbox{Pr}(x>40)}{\mbox{Pr}(x>30)} \\ \\
\end{align}
$$

![[Pasted image 20230506233800.png]]

So 
$$
\frac{5/25}{15/25}=\frac{5}{15}=\frac{1}{3}=33.3%
$$ 

---

### References

[Continuous Probability Uniform Distribution Problems - YouTube](https://www.youtube.com/watch?v=KfunVw-0AH0&list=PL0o_zxa4K1BVsziIRdfv4Hl4UIqDZhXWV&index=42)