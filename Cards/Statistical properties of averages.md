up::  [[Probability and Statistics]]
tags:: #source/book #note/reference #on/Statistics 
X:: 

## Statistical properties of averages

#### The expected value of the sum of random variables is the sum of each random variable’s expected value.

$$
\mbox{E}[X_1+X_2+\dots+X_n] =  \mbox{E}[X_1] + \mbox{E}[X_2]+\dots+\mbox{E}[X_n]
$$
If $X$ are independent draws, then they all have the same expected value, which is $\mu$ in:
$$
\mbox{E}[X_1+X_2+\dots+X_n]=  n\mu
$$


#### The expected value of a non-random constant times a random variable is the non-random constant times the expected value of a random variable. 

$$
\mbox{E}[aX] =  a\mbox{E}[X]
$$
#### The square of the standard error of the sum of independent random variables (aka __variance__) is the sum of the square of the standard error of each random variable.

$$
\mbox{SE}[X_1+X_2+\dots+X_n] = \sqrt{\mbox{SE}[X_1]^2 + \mbox{SE}[X_2]^2+\dots+\mbox{SE}[X_n]^2  }
$$

But I think it's easier to understand:

$$
{\mbox{SE}[X_1+X_2+\dots+X_n]}^2 = \mbox{SE}[X_1]^2 + \mbox{SE}[X_2]^2+\dots+\mbox{SE}[X_n]^2
$$
#### The standard error of a non-random constant times a random variable is the non-random constant times the random variable’s standard error. As with the expected value:

$$
\mbox{SE}[aX] =  a\mbox{SE}[X]
$$

#### For a random variable $X$ and non-random constants $a$ and $b$, then if $X$ is normally distributed then $aX+b$ is too.

#### These are all hallmarks of linearity!


---

### References

[[Introduction to Data Science]]



