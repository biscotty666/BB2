up:: [[Introduction to Data Science]]
tags:: #source/book  #on/R #on/DataVisualization #note/reference 
X:: [[Data Visualization]]

## Compare differences versus average with Bland-Altman plots

In this case we use a scatterplot with differences on the y axis and averages on the x axis.

![[Pasted image 20230408224611.png]]

```
library(ggrepel)
dat |> 
  mutate(year = paste0("life_expectancy_", year)) |>
  select(country, year, life_expectancy) |> 
  pivot_wider(names_from = "year", 
              values_from = "life_expectancy") |> 
  mutate(average = (life_expectancy_2015 + life_expectancy_2010)/2,
         difference = life_expectancy_2015 - life_expectancy_2010) |>
  ggplot(aes(average, 
             difference, 
             label = country)) + 
  geom_point() +
  geom_text_repel() +
  geom_abline(lty = 2) +
  xlab("Average of 2010 and 2015") + 
  ylab("Difference between 2015 and 2010")
```


---
### References

