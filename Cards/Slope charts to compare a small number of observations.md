up:: [[R]]
tags:: #note/reference #on/R #on/DataVisualization 
X:: [[Data Visualization]]

## Slope charts to compare a small number of observations

A slope chart can be used instead of a scatter plot in this case. The change is visually encoded in the angle. Eg.

![[Pasted image 20230408224110.png]]

R does not have a `geom_slopeplot` so some extra coding is necessary.

```
gapminder |>
  filter(year %in% c(2010, 2015) & 
         region %in% west &
         !is.na(life_expectancy) &
         population > 10^7) -> 
  dat

dat |>
  mutate(location = ifelse(year == 2010, 1, 2),
         location = ifelse(year == 2015 &
                             country %in% c("United Kingdom", "Portugal"),
                           location + 0.22,
                           location),
         hjust = ifelse(year == 2010, 1, 0)) |>
  mutate(year = as.factor(year)) |>
  ggplot(aes(year, 
             life_expectancy, 
             group = country)) +
  geom_line(aes(color = country), 
            show.legend = FALSE) +
  geom_text(aes(x = location, 
                label = country, 
                hjust = hjust),
            show.legend = FALSE) +
  xlab("") + ylab("Life Expectancy")
```

---
### References

