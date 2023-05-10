up:: [[Syntax ]]
tags:: #note/reference #on/R #on/Python #on/Syntax
X:: 

## Replacement based on a substring

[[R]]:

```
survey_df |> 
  mutate(Gender = if_else(grepl(";", Gender), NA, Gender))
```

[[Python]]

```
survey_df.where(~(survey_df.Gender.str.contains(';', na=False)), np.nan, inplace=True)
```



---
### References
