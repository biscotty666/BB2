up:: [[Syntax ]]
tags:: #note/reference #on/R #on/Python #on/Syntax
X:: 

## Convert column to numeric

[[R]]:

```
survey_df |>
  mutate(
    Age1stCode = as.numeric(Age1stCode),
    YearsCode = as.numeric(YearsCode),
    YearsCodePro = as.numeric(YearsCodePro)
  ) -> survey_df
```

[[Python]]

```
survey_df['Age1stCode'] = pd.to_numeric(survey_df.Age1stCode, errors='coerce') survey_df['YearsCode'] = pd.to_numeric(survey_df.YearsCode, errors='coerce') survey_df['YearsCodePro'] = pd.to_numeric(survey_df.YearsCodePro, errors='coerce')
```



---
### References

