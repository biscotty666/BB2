up:: [[R]]
tags:: #note/reference  #on/R  
X:: 

## Create a factor column in a data frame

In [[R]] use the `factor()` command, eg:

```
gdp <- c(14.6, 5.7, 5.3, 3.3, 2.5)
gdp_data <- data.frame(Country = rep(c("United States", "China",
                                       "Japan", "Germany",
                                       "France"),
                                     2),
           y = factor(rep(c("Radius","Area"),each=5), 
                      levels = c("Radius", "Area")),
           GDP= c(gdp^2/min(gdp^2), gdp/min(gdp))) |%3E 
   mutate(Country = reorder(Country, GDP))
```

And here is an interesting chart:

```
gdp_data |> 
  ggplot(aes(Country, y, size = GDP)) + 
  geom_point(show.legend = FALSE, color = "blue") + 
  scale_size(range = c(2,25)) +
  coord_flip() + ylab("") + xlab("")
```

![[Pasted image 20230408220311.png]]
