up:: [[Syntax ]]
tags:: #note/reference #on/R #on/Python #on/Syntax
X:: 

## Histograms in R and Python

[[R]]:

```
survey_df |>
  ggplot(aes(Age)) +
  geom_histogram(binwidth = 5, colour = "black", fill="purple") +
  scale_x_continuous(name = "Age",
                   breaks = seq(10, 80, 10)) +
  xlim(10, 80) +
  ylab("Number of respondents")
```

[[Python]]

```
plt.figure(figsize=(12, 6))
plt.title(schema.Age)
plt.xlabel('Age')
plt.ylabel('Number of respondents')

plt.hist(survey_df.Age, bins=np.arange(10,80,5), color='purple');>)
```



---
### References
