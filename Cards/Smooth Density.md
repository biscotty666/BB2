up:: [[Data Visualization]]
tags:: #source/book #note/reference #on/Visualization 
X:: [[Introduction to Data Science]] [[Probability and Statistics]]

## Smooth Density

Similar to a histogram, but it plots the density (points) instead of the bars. This can be easier to understand than a histogram.

![[Pasted image 20230413085718.png]]

In [[R]] the _"smoothness"_ of the density line can be adjusted with the `adjust` parameter. 

```
tmp <-
  heights |> 
  ggplot(aes(height)) +
  geom_histogram(aes(y = ..density..),
                 binwidth = 1,
                 alpha = 0.5)
p1 <- tmp +
  geom_line(stat = "density", adjust = 0.5)
p2 <- tmp +
  geom_line(stat = "density", adjust = 2)

grid.arrange(p1, p2, ncol=2)
```

![[Pasted image 20230413090302.png]]
