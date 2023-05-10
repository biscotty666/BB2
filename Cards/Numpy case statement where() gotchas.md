up:: [[Python]]
tags:: #note/reference #on/Python #on/Syntax #note/gotcha
X:: [[Syntax]]

## Numpy case statement where() gotchas

1. Must use bitwise operators, ie. `&` and `|` rather than  `and` and `or`
2. Must use parentheses around each part

```
survey_df['AgeGroup'] = np.where(survey_df['Age'] %3C 18, "Younger than 18",
                        np.where((survey_df['Age'] %3E= 18) & 
                                 (survey_df['Age'] <= 25), "18-25 years",
                        np.where((survey_df['Age'] > 25) & 
                                 (survey_df['Age'] <= 35), "26-35 years",
                        np.where((survey_df['Age'] > 35) & 
                                 (survey_df['Age'] <= 45), "36-45 years",
                        np.where((survey_df['Age'] > 45) & 
                                 (survey_df['Age'] <= 55), "46-55 years",
                        "Older than 55")))))
```

---
### References

