up:: [[Gamma distribution]]
tags:: #on/PnS #source/video #note/example #note/reference  

## Bakery Customers

Customers arrive according to a Poisson process at an average rate of 15 per hour.

1. What is the probability that it takes less than 10 minutes before the first 3 customers arrive?

At an average rate of 15/hour, 1 arrives every 4 minutes on average, so $\theta=4$. Since we want 3 occurences, $\alpha=3$. If $X$ is the waiting time for the third customer:
$$
\begin{align}
\mbox{P}(X\le 10) &= \int_0^{10}\frac{x^{3-1}e{-x/4}}{(3-1)!4^3}dx \\ \\
&\approx 0.456
\end{align}
$$

2. What is the average amount of time that will elapse before 3 customers arrive?

This is the expected value with $\theta=4$ and $\alpha=3$

$$
\begin{align}
\mu&=/theta/alpha \\ 
&=(4)(3) \\ 
&=12\text{ minutes}
\end{align}
$$
3. What is the probability that exactly 15 customers arrive in an hour?
Since we are considering the number of customers, which is a _discrete random variable_ we cannot use the gamma distribution. Instead we need to use the [[Poisson distribution]].

For the average number of occurrences $\lambda$ and the number of occurrences $x$:
$$
\begin{align}
\mbox{P}(X=x)&=\frac{\lambda^xe^{-\lambda}}{x!} \\ \\
\mbox{P}(X=15)&=\frac{15^{15}e^{-15}}{15!} \\ \\
&\approx 0.102
\end{align}
$$


---

### References

[The gamma distribution - YouTube](https://www.youtube.com/watch?v=cpW40zPdAQ8)