up:: [[R]]
tags:: #note/reference #on/R
X:: 

## Remove rows with NA values

Use `omit()`, eg:

```
survey_df |%3E
  na.omit() |>
  summarise(Age = n(), .by = Gender)
```

Or in tidyverse use `drop_na()`:
```
survey_df |>
  drop_na() |>
  summarise(Age = n(), .by = Gender)
```

---
### References

