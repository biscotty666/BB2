up:: [[Syntax ]]
tags:: #note/reference #on/R #on/Python #on/Syntax
X:: 

## Bar chart with counts

[[R]]:

```
top_countries |%3E
  ggplot(aes(x = Country, y = Count, fill = Country)) +
  geom_bar(stat = "identity", show.legend = FALSE) +
  ggtitle(country_question) +
  theme(axis.text.x = element_text(angle=75, vjust=1, hjust = 1))
```

[[Python]]

```
plt.figure(figsize=(12,6))
plt.xticks(rotation=75)
plt.title(schema.Country)
sns.barplot(x=top_countries.index, y=top_countries)
```



---
### References
