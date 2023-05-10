up:: [[Box and Whiskers Plot]] 
tags:: #note/reference #on/R 
X:: 

## Change the length of the whiskers in a box plot

The whiskers in [[R]] are by default 1.5 times the Inter-quartile range (IQR). This can be changed with the `coef` argument to `geom_boxplot`. Eg:

```

<heights |>
  ggplot(aes(sex, height)) +
  geom_boxplot(coef = 3) +
  geom_jitter(width = 0.1, alpha = 0.2) +
  ylab("Height in inches")
```



