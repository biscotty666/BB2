up:: [[R]]
tags:: #note/reference #on/R #on/DataVisualization 
X:: [[Data Visualization]]

## Pie Charts in R

Pie charts in [[R]] are bar charts with polar coordinates. eg.

```
ggplot(aes(x="", y=Percentage, fill = Browser)) +
	geom_bar(width = 1, stat = "identity", col = "black") +
	coord_polar(theta = "y")
```

or, more simply with `geom_col()`:

```
survey_df |%3E
  drop_na() |>
  summarise(Age = n(), .by = Gender) |>
  mutate(Percentage = Age / sum(Age)) |>
  mutate(labels = scales::percent(Percentage)) |>
  ggplot(aes(x="", y=Percentage, fill = Gender)) +
	geom_col() +
	coord_polar(theta = "y") +
  geom_text(aes(label = labels), position = position_stack(vjust = 0.5)) +
  theme(axis.text=element_blank(), 
        axis.ticks = element_blank(), 
        panel.grid  = element_blank()) +
  xlab("") +
  ylab("Percentage of responses")
```

NB **Pie charts are not usually a good idea** because they don't convey much information. Also, people have difficulty judging relative areas but can easily judge relative lengths.

---
### References

