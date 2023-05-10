up:: [[R]]
tags:: #note/reference #on/R #on/DataVisualization 
X:: [[Data Visualization]]

## Ridge Plots

Stacked smooth density or histograms. In [[R]] use `geom_density_ridges()`

Example:

[10 Data visualization in practice](https://biscotty666.github.io/Data-Science-R-PH125x/docs/Pt10.html#ridge-plots)

A beautiful example with faceting:

```
gapminder |>
  filter(year %in% years & country %in% country_list) |>
  group_by(year) |>
  mutate(weight = population/sum(population)*2) |>
  ungroup() |>
  ggplot(aes(dollars_per_day, fill = group)) +
  scale_x_continuous(trans = "log2", limit = c(0.125, 300)) + 
  geom_density(alpha = 0.2, bw = 0.75, position = "stack") + 
  facet_grid(year ~ .)
```

![[Pasted image 20230407220934.png]]
