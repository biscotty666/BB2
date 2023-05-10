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



---

### References

[The gamma distribution - YouTube](https://www.youtube.com/watch?v=cpW40zPdAQ8)