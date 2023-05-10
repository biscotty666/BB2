up:: [[R]]
tags:: #note/reference #on/R #on/Syntax
X:: [[Syntax ]]

## Set or query graphical parameters

`par()` takes various parameters:

- `mfrow = c(nr, nc)`: sets the number of rows and columns for display of multiple graphs
- `mar = c(bottom, left, top, right`: sets the margins 
- `mgp = c(axis title, axis labels, axis line`

In [[Introduction to Data Science]] we used:


```
par(mfrow=c(2,2), 
	mar = c(3, 1, 3, 0), 
	mgp = c(1.5, 0.5, 0)) 
take_poll(25); take_poll(25); take_poll(25); take_poll(25)
```



---
### References

[15 Statistical inference](https://biscotty666.github.io/Data-Science-R-PH125x/docs/Pt15.html)