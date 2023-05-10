up:: [[Probability and Statistics]]
tags:: #source/book #note/reference #on/Probability 
X:: [[Expected Value Roulette]] [[Expected value of discrete variables]]

## Expected Value and Standard Error

The mean of [[Random variables]] is called the __expected value__. The standard deviation is called the __standard error__.

The expected value is denoted $E[X]$  where $X$ is a random variable. With two possible outcomes $a$ and $b$ and probabilities $p$ and $(1-p)$, the _average_ is:

$$
\mbox{E}[X] = ap + b(1-p)
$$

So the expected value is

$$\text{number of draws} \times \text{average of the numbers in the urn}$$

$$
\mbox{E}[X] = (ap + b(1-p))\times N
$$

The _standard error of the sum_, assuming _independence_, is given by:

$$\sqrt{\text{number of draws}}\times \text{standard deviation of numbers in urn}$$
So 

$$
SE_{\text{sum}}=\mid b - a \mid \sqrt{p(1-p)}.
$$

The sampling distribution of the mean refers to the distribution of the means of repeated randomly generated samples ([[Random variables]]). The sampling distribution itself has a mean and variance (SE). The larger the sample size the closer SE is to SD.

$$
SE_{mean}=\frac{standard\ deviation}{square\ root\ of\ sample\ size}
$$
The standard error on the mean is designated by $\sigma_{\hat{x}}$ or $s$.  So:

$$
\sigma_{\hat{x}}=\frac{\sigma}{\sqrt{n}}
$$

---
### References

[14 Random variables](https://biscotty666.github.io/Data-Science-R-PH125x/docs/Pt14.html#population-sd-versus-the-sample-sd)

[en.wikipedia.org/wiki/Standard_error](https://en.wikipedia.org/wiki/Standard_error)