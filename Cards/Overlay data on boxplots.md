up:: [[R]]
tags:: #note/reference #on/R #on/DataVisualization 
X:: [[Data Visualization]]

## Overlay data on boxplots

An important principle in [[Data Visualization]] is to __show your work__. Boxplots can be enhanced by showing the data points. In [[R]]:

```
heights |%3E
  ggplot(aes(sex, height)) +
  geom_boxplot(coef = 3) +
  geom_jitter(width = 0.1, alpha = 0.2) +
  ylab("Height in inches") -> p3
p3
```

Produces:

![[Pasted image 20230408223134.png]]

---
### References

