up:: [[Probability and Statistics]]
tags:: #on/PnS #source/video #source/StatQuest #note/reference 

## Maximum likelihood of the Exponential Distribution

The goal of maximum likelihood is, given a set of measurements, find an optimal value for $\lambda$.

The likelihood of any measurement $x$ is given by

$$
\mbox{L}(\lambda\,|\,x)=\lambda e^{-\lambda x}
$$
The case of two measurements would be

$$
\begin{align}
\mbox{L}(\lambda\, |\,x_1\, \text{and}\, x_2) &=\mbox{L}(\lambda\, |\, x_1)\mbox{L}(\lambda\, |\, x_2) \\
&=\lambda e^{-\lambda x_1}\lambda e^{-\lambda x_2} \\
&=\lambda^2[e^{-\lambda(x_1+x_2)}]
\end{align}
$$

The likelihood of $\lambda$ given all the data

$$
\mbox{L}(\lambda\, |\,x_1,x_2,\dots,x_n)=\lambda^n[e^{-\lambda(x_1+x_2+\dots+x_n)}]
$$

To find the maximum likelihood, take the derivative with respect to $\lambda$ and set equal to 0.


![[Pasted image 20230426234800.png]]

![[Pasted image 20230426234851.png]]

For example, assume 2 seconds passed between the first time the video was watched and the second time, then 2.5 seconds passed until the next time, then 1.5 seconds. So $x_1=2$, $x_2=2.5$ and $x_2=1.5$.

$$
\begin{align}
\lambda &= \frac{n}{(x_1+x_2+x_3)} \\
&= \frac{3}{(2+2.5+1.5)} \\ \\
\lambda &= 0.5
\end{align}
$$

![[Pasted image 20230426232247.png]]

![[Pasted image 20230426232417.png]]


---

[Maximum Likelihood for the Exponential Distribution, Clearly Explained!!! - YouTube](https://www.youtube.com/watch?v=p3T-_LMrvBc&t=0s)
[Statistics Fundamentals - YouTube](https://www.youtube.com/playlist?list=PLblh5JKOoLUK0FLuzwntyYI10UQFUhsY9)