up:: [[Syntax ]]
tags:: #note/reference #on/R #on/Python #on/Syntax
X:: 

## Select top n number of rows

[[R]]:

```
survey_df |>
  summarise(Count = n(), .by = Country) |>
  top_n(n = 15, wt = Count) |>
  mutate(Country = reorder(Country, Count, decreasing = TRUE)) -> top_countries
```

[[Python]]

```
top_countries = survey_df.Country.value_counts().head(15)
top_countries
```



---
### References
