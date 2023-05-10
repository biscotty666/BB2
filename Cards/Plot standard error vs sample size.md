up:: [[R]]
tags:: #note/reference #on/R #on/Syntax
X:: [[Syntax ]]

## Plot standard error vs sample size


```
p <- 0.51
N <- 10^seq(1,5, len=100)
data.frame(N=N, 
           SE = sqrt(p*(1-p)/N)) |> 
  ggplot(aes(N, SE)) + 
  geom_line() + 
  scale_x_continuous(breaks = c(10,100,1000,10000), 
                     trans = "log10")
```



---
### References
