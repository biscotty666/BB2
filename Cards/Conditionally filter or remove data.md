up:: [[Syntax ]]
tags:: #note/reference #on/R #on/Python #on/Syntax
X:: 

## Conditionally filter or remove data

These are close but not exactly the same. #note/investigate🔎 

[[R]]:

```
survey_df |>
  filter(Age >= 10 | is.na(Age)) |>
  filter(WorkWeekHrs %3C= 140 | is.na(WorkWeekHrs))
```

[[Python]]

```
survey_df.drop(survey_df[survey_df.Age < 10].index, inplace=True)
survey_df.drop(survey_df[survey_df.Age > 100].index, inplace=True)
survey_df.drop(survey_df[survey_df.WorkWeekHrs > 140].index, inplace=True)
```



---
### References
