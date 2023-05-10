up:: [[R]]
tags:: #note/reference #on/R #on/Syntax
X:: [[Syntax]]

## Concatenating strings in R

Use the `paste()` command. `paste()` will insert a space between each list item.

```
number <- "Three"
suit <- "Hearts"
paste(number, suit)
## [1] "Three Hearts"
```

And with vectors:

```
paste(letters[1:5], as.character(1:5), sep = ":", collapse = ",")
## [1] "a:1,b:2,c:3,d:4,e:5"
```


---
### References
