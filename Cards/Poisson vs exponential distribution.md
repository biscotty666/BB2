up:: [[Probability and Statistics]]
tags::  #on/Probability #note/reference #source/video 
related:: [[Poisson distribution]]

## Poisson vs exponential distribution

### Exponential

- Time between two events
- Continuous interval of time
- Times between events are independently exponentially distributed with parameter $\lambda$
$$
\begin{align}
\mbox{P}(X<x) &=1-e^{-\lambda x} \\
\mbox{P}(X>x) &=e^{-\lambda x}
\end{align}
$$
- Question: what is the probability of an event occurring in a certain time interval, eg. what is the probability of completing an assignment between 5 and 8 minutes?
- In the above question, $\lambda=\frac{6}{60}=0.01$, expressing per/minute.
$$
\begin{align}
\mbox{P}(5<X<8)&=\mbox{P}(x<8)-\mbox{P}(x<5) \\
&=1-e^{-0.1(8)} - [1-e^{-0.1(5)}] \\
&=e^{-0.1(5)}-e^{-0.1(8)} \\
&=0.1572
\end{align}
$$

### Poisson

[[Poisson Examples]]

- Number of events in a time interval
- Discrete time between events
- Number of events in one unit of time has a Poisson distribution with parameter $\lambda$.
$$
\mbox{P}(X=x)=\frac{e^{-\lambda}\lambda^x}{x!}
$$
- Question: What is the probability of two events happening in a certain time period, eg. what is the probability of completing less than two problems in 20 minutes?
- In the above problem $\lambda=2$, expressing per/20 minutes. 
- Note that the value of $\lambda$ is the same as in Exponential, only the units are different

$$
\begin{align}
\mbox{P}(X<2)&=\mbox{P}(X=0)+\mbox{P}(X=1) \\
&=\frac{e^{-2}2^0}{0!}+\frac{e^{-2}2^1}{1!} \\
&=.4060
\end{align}
$$
![[Pasted image 20230427163606.png]]

![[Pasted image 20230427163504.png]]



---

### References
https://youtu.be/Z-8FtjZNlb4
[Poisson distribution - Wikipedia](https://en.wikipedia.org/wiki/Poisson_distribution)
[Normal distribution - Wikipedia](https://en.wikipedia.org/wiki/Normal_distribution)