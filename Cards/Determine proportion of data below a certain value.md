up:: [[Probability and Statistics]]
tags:: #source/book #note/reference #on/R #on/Statistics 
X:: [[R]] 

## Determine proportion of data below a certain value

Use the __empirical cumulative distribution function (eCDF)__. 

```
ggplot(aes(x)) +
  stat_ecdf()
```
