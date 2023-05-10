up:: [[Binomial Distribution]]
tags:: #on/Probability  #source/video #source/StatQuest #note/reference 

## Binomial Distribution Coin Flip

Let $x$ be the number of heads from flipping coins 5 times. There are $2^5$ or 32 possible outcomes from 5 flips. Note that this is a _discrete random variable_.

$$
\begin{align}
P(x=0)=\frac{1}{32}=\frac{_5C_0}{32}&;\; _5C_0=\frac{5!}{0!(5-0)!}=\frac{5!}{5!}=1 \\ \\
P(x=1)=\frac{_5C_1}{32}&;\; _5C_1=\frac{5!}{1!(5-1)!}=\frac{5!}{4!}=5 \\ \\
P(x=2)=\frac{_5C_2}{32}&;\; _5C_2=\frac{5!}{2!(5-2)!}=\frac{5\cdot4\cdot3\cdot2}{2\cdot3\cdot2}=10 \\ \\
P(x=3)=\frac{_5C_3}{32}&;\; _5C_3=\frac{5}{3!(5-3)!}=\frac{5!}{3!2!}=10 \\ \\
P(x=4)=\frac{_5C_4}{32}&;\; _5C_4=\frac{5}{4!(5-4)!}=5 \\ \\
P(x=5)=\frac{_5C_5}{32}&;\; _5C_5=\frac{5}{5!(5-5)!}=\frac{5!}{5!}=1
\end{align}
$$
|      | P(x=0) | P(x=1) | P(x=2) | P(x=3) | P(x=4) | P(x=5) |
| ---- | ------ | ------ | ------ | ------ | ------ | ------ |
| Number of 32nd intervals | 1      | 5      | 10     | 10     | 5      | 1      | 
^table

```chart
type: bar
id: table
layout: rows
width: 80%
beginAtZero: true
```


---

### References

[Statistics - Binomial & Poisson Distributions - YouTube](https://www.youtube.com/watch?v=BR1nN8DW2Vg)