---
title: "Visualization: Gini Index"
layout: post
image: /assets/images/favicon/GitHub-Mark.png
headerImage: false
tag:
- gganimate
- bar chart
- Visualization
- Sapply
projects: true
category: blog
author: subaiwen
description: Data Visualization for Gini Index
---

## Overview:
Visualization for data from [World Bank, Development Research Group](https://data.worldbank.org/indicator/si.pov.gini). 
![](https://tva1.sinaimg.cn/large/006y8mN6ly1g6my3kmm1wj313c0biwkj.jpg)

## Manipulation
```r
n = 1970
while (n < 2015){
  df[,paste0(n,"-",n+4)] = rowMeans(df[,as.character(n:(n+4))], na.rm = TRUE)
  n = n + 5
}
df[,paste0(2015,"-",2017)] = rowMeans(df[,as.character(2015:2017)], na.rm = TRUE)
```
Create columns for 5-year time span by `while loop` and `rowMeans`.

## Visualization
### Option1: Animation
Gather the columns for `Year`, replace `NA` with `0` and then plot it.

```r
library(gganimate)
df.anime <- df %>% gather(key = "Year", value = "Gini", as.character(1970:2017)) %>% select(Country, Year, Gini)
df.anime[,-1] %>% sapply(function(x){ifelse(is.na(x),0,x)}) %>% sapply(as.numeric) -> df.anime[,-1]
a <- df.anime %>%
  ggplot(aes( x = Country, y = Gini, fill = Country)) +
  geom_bar(stat = "identity") +
  coord_flip() +
  labs(title = 'Year: {frame_time}', x = 'Gini index', y = 'Country') +
  transition_time(as.integer(Year)) +
  ease_aes('linear')
animate(a, fps=4)
anim_save("~/animation.gif")
``` 
![](https://subaiwen.github.io/assets/animation.gif)  
Fancy but not informative.

### Option2: Bar chart
To make bar chart ordered in each facet, we need to [set up](https://github.com/dgrtwo/drlib/blob/master/R/reorder_within.R) a little bit.  

```r
df.bnime <- df %>% gather(key = "Year", value = "Gini", (ncol(df)-9):ncol(df)) %>% select(Country, Year, Gini)
df.bnime[,3] %>% sapply(function(x){ifelse(is.nan(x),0,x)}) %>% sapply(as.numeric) -> df.bnime[,3]
### set up
reorder_within <- function(x, by, within, fun = mean, sep = "___", ...) {
  new_x <- paste(x, within, sep = sep)
  stats::reorder(new_x, by, FUN = fun)
}
scale_x_reordered <- function(..., sep = "___") {
  reg <- paste0(sep, ".+$")
  ggplot2::scale_x_discrete(labels = function(x) gsub(reg, "", x), ...)
}
### graph
df.bnime %>%
  ggplot(aes( reorder_within(Country,Gini,Year), Gini, fill = Country)) +
  geom_bar(stat = "identity") +
  coord_flip() +
  scale_x_reordered() +
  labs(x = 'Gini index', y = 'Country') +
  facet_wrap(~Year, scales = "free")
```
![](https://tva1.sinaimg.cn/large/006y8mN6ly1g6myh3cbxaj312l0u01j7.jpg)  
Much better!

---

### Reference
[gganimate](https://gganimate.com)  
[Reorder an x or y axis within facets](https://github.com/dgrtwo/drlib/blob/master/R/reorder_within.R)

