up:: [[Binomial Distribution]]
tags:: #on/PnS #source/video #note/reference  

## Expected Value of Binomial Distribution

$$
X=\text{\# of successes with a probability }p\text{ in }n\text{ trials}
$$
$$E(X)=n\cdot p
$$
Let $X$ be the number of baskets I make after 10 shots with at a  probability of 40%. Then the expected value is
$$
E(X)=10\cdot 0.4=4
$$
The probability of getting $k$ successes would be the binomial coefficient times the probability of winning times the probability of losing.
$$
P(X=k)=\binom{n}{k}p^k(1-p)^{n-k}
$$
The expected value of a [[Random variables|random variable]] is the probability weighted sum.
$$
E(X)=\sum_{k=0}^{n}k\binom{n}{k}p^k(1-p)^{n-k}
$$
Since the first term is 0 we can re-write:
$$
\begin{align}
E(X)&=\sum_{k=1}^{n}k\binom{n}{k}p^k(1-p)^{n-k} \\ \\
&=\sum_{k=1}^{n}k\frac{n!}{k!(n-k)!}p^k(1-p)^{n-k}  \\ \\
&=\sum_{k=1}^{n}\frac{n!}{(k-1)!(n-k)!}p^k(1-p)^{n-k} \\ \\
&=\sum_{k=1}^{n}\frac{n\cdot (n-1)!}{(k-1)!(n-k)!}\cdot p\cdot p^{k-1}(1-p)^{n-k} \\ \\
&=np\sum_{k=1}^{n}\frac{(n-1)!}{(k-1)!(n-k)!}p^{k-1}(1-p)^{n-k} \\ \\
\end{align}
$$
Let $a=k-1$ and $b=n-1$. Then $n-k=b-a$.
$$
\begin{align}
E(X)&=np\sum_{a=0}^b\frac{b!}{a!(b-a)!}p^a(1-p)^{b-a} \\ \\
&=np\sum_{a=0}^b\binom{b}{a}p^a(1-p)^{b-a} \\ \\
\end{align}
$$
Since the sum of all probabilities _in the binomial distribution_ is 1, we have 
$$
E(X)=n\cdot p
$$

---

### References

[Expected value of binomial distribution | Probability and Statistics | Khan Academy - YouTube](https://www.youtube.com/watch?v=SqcxYnNlI3Y&list=PLSQl0a2vh4HCLjAoORNwUJLWi5ovMjbax&index=11)