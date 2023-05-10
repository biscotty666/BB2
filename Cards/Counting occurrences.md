up:: [[Syntax ]]
tags:: #note/reference #on/R #on/Python #on/Syntax
X:: 

## Counting occurrences

[[R]]:

```
survey_df |>
  summarise(Age = n(), .by = Gender)
```

[[Python]]

```
survey_df['Gender'].value_counts()
```



---
### References
