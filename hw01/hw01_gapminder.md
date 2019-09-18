---
title: "hw01_gapminder"
output:
  html_document:
    keep_md: true
---

## Loading the Dataset

R has a dataset called gapminder, which the following code loads, along with ggplot2 for plotting graphs:
 

```r
library(gapminder)
library(ggplot2)
```


## Variables

Here are the variables for gapminder after running the following code:


```r
names(gapminder)
```

```
## [1] "country"   "continent" "year"      "lifeExp"   "pop"       "gdpPercap"
```

## Rows

Here are the number of rows in gapminder after running the following code:


```r
nrow(gapminder)
```

```
## [1] 1704
```

## Dataset Summary

Here is a summary of statistics of gapminder after running the following code:


```r
summary(gapminder)
```

```
##         country        continent        year         lifeExp     
##  Afghanistan:  12   Africa  :624   Min.   :1952   Min.   :23.60  
##  Albania    :  12   Americas:300   1st Qu.:1966   1st Qu.:48.20  
##  Algeria    :  12   Asia    :396   Median :1980   Median :60.71  
##  Angola     :  12   Europe  :360   Mean   :1980   Mean   :59.47  
##  Argentina  :  12   Oceania : 24   3rd Qu.:1993   3rd Qu.:70.85  
##  Australia  :  12                  Max.   :2007   Max.   :82.60  
##  (Other)    :1632                                                
##       pop              gdpPercap       
##  Min.   :6.001e+04   Min.   :   241.2  
##  1st Qu.:2.794e+06   1st Qu.:  1202.1  
##  Median :7.024e+06   Median :  3531.8  
##  Mean   :2.960e+07   Mean   :  7215.3  
##  3rd Qu.:1.959e+07   3rd Qu.:  9325.5  
##  Max.   :1.319e+09   Max.   :113523.1  
## 
```

## Life Expectancy vs. Year

Plot of lifeExp vs. year using the following code:


```r
ggplot(gapminder, aes(year, lifeExp)) +
  geom_line(colour = "blue")
```

![](hw01_gapminder_files/figure-html/unnamed-chunk-4-1.png)<!-- -->
