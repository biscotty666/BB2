up:: [[Binomial Distribution]]
tags:: #on/PnS #source/video #note/example #note/reference  

## Twitter Example

There is a 0.85 probability that a randomly selected person know what Twitter is.

What is the probability that exactly 3 in 5 randomly selected adults know of Twitter?

Does the procedure result in a binomial distribution? (Yes)

$$
P(x)=\frac{n!}{(n-x)!x!}\cdot p^x\cdot q^{n-x}
$$
In this case
$$
p^x\cdot q^{n-x} = 0.85\times0.85\times0.85\times 0.15\times 0.15
$$
$$
\begin{align}
P(3)&=\frac{5!}{(5-3)!3!}\times 0.85^3\times 0.15^{(5-3)} \\ \\
&=\frac{5!}{2!3!}\times 0.614125\times 0.0225 \\ \\
&=0.138
\end{align}
$$


---

### References

[Statistics - Binomial & Poisson Distributions - YouTube](https://www.youtube.com/watch?v=BR1nN8DW2Vg)