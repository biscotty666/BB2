up:: [[Outliers]]
tags:: #source/book #note/reference #on/Visualization #on/Statistics 
X:: [[R]] 

## Case study self-reported student heights

```
library(dslabs)
data("reported_heights")
str(reported_heights)
```

```
reported_heights <- 
  reported_heights |>
  mutate(original_heights = height,
         height = as.numeric(height))
```

```
reported_heights <- filter(reported_heights, !is.na(height))
```

```
reported_heights |>
  group_by(sex) |>
  summarise(average = mean(height), sd = sd(height),
            median = median(height), MAD = mad(height))
```


|sex|average|sd|median|mad|
|---|---|---|---|---|
|chr|dbl|dbl|dbl|dbl|
|Female|63.4|27.9|64.2|4.05|
|Male|103.4|529.9|70.0|4.45|

