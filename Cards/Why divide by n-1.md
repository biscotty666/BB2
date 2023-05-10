up:: [[Probability and Statistics]]
tags:: #on/PnS #source/video #source/StatQuest #note/reference 
related:: [[Population Variance]]

## Why divide by n-1

$$
\frac{\sum(x-\bar{x})^2}{n} \lt \frac{\sum(x-\mu)^2}{n}
$$

So by using $n-1$ in the denominator we come closer to the actual population variance, which is our goal. To compensate reduce the denominator on the left by 1.

### Explanation

Consider the variance around $v$.
$$
\frac{\sum (x-v)^2}{n}
$$
Find the value of $v$ with the least variance:
$$
\begin{align}
\frac{d}{dv} \frac{\sum (x-v)^2}{n} &= \frac{\sum 2(x-v)(-1)}{n} \\
&= \frac{-2}{n} \sum (x-v)
\end{align}
$$
Set the derivative to 0:
$$
\begin{align}
0 &= \frac{-2}{n} \sum (x-v) \\
&=\frac{-2}{n}[(x_1-v)+(x_2-v)+\dots+(x_n-v)] \\
&=[(x_1-v)+(x_2-v)+\dots+(x_n-v)] \\[1em]
v &= \frac{x_1+x_2+\dots+x_n}{n} \\
&= \bar{x}
\end{align}
$$

__So the value that gives the least variance is $\bar{x}$.__

![[Pasted image 20230428160813.png]]

![[Pasted image 20230428160420.png]]

![[Pasted image 20230428160945.png]]

---

[Why Dividing By N Underestimates the Variance - YouTube](https://www.youtube.com/watch?v=sHRBg6BhKjI&t=0s)
[Statistics Fundamentals - YouTube](https://www.youtube.com/playlist?list=PLblh5JKOoLUK0FLuzwntyYI10UQFUhsY9)