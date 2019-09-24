--- 
title: "hw02_dplyr"
output:
  html_document:
    keep_md: yes
  pdf_document: default
---    
    


Load tidyverse, gapminder, and datasets:


```r
library(tidyverse)
library(gapminder)
library(datasets)
```

## 1.1

Gapminder dataset filtered for data in only the 1970's:

```r
gapminder %>%
  filter(year == 1970:1979)
```

```
## # A tibble: 57 x 6
##    country                  continent  year lifeExp       pop gdpPercap
##    <fct>                    <fct>     <int>   <dbl>     <int>     <dbl>
##  1 Albania                  Europe     1977    68.9   2509048     3533.
##  2 Argentina                Americas   1972    67.1  24779799     9443.
##  3 Austria                  Europe     1977    72.2   7568430    19749.
##  4 Belgium                  Europe     1972    71.4   9709100    16672.
##  5 Bolivia                  Americas   1977    50.0   5079716     3548.
##  6 Brazil                   Americas   1972    59.5 100840058     4986.
##  7 Burkina Faso             Africa     1977    46.1   5889574      743.
##  8 Cameroon                 Africa     1972    47.0   7021028     1684.
##  9 Central African Republic Africa     1977    46.8   2167533     1109.
## 10 China                    Asia       1972    63.1 862030000      677.
## # … with 47 more rows
```

3 countries picked from the filtered results and refiltered for Greece, Austria, and Philippines within the 1970's.  A new table 'gapminder1.1' is made:

```r
gapminder1.1 <- (gapminder %>%
  filter(country == 'Greece' | country == 'Austria' | country == 'Philippines', year == 1970:1979))
gapminder1.1
```

```
## # A tibble: 3 x 6
##   country     continent  year lifeExp      pop gdpPercap
##   <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
## 1 Austria     Europe     1977    72.2  7568430    19749.
## 2 Greece      Europe     1972    72.3  8888628    12725.
## 3 Philippines Asia       1977    60.1 46850962     2373.
```

## 1.2 

Variables country and gdpPercap only shown from gapminder1.1:

```r
gapminder1.1 %>%
  select(country, gdpPercap)
```

```
## # A tibble: 3 x 2
##   country     gdpPercap
##   <fct>           <dbl>
## 1 Austria        19749.
## 2 Greece         12725.
## 3 Philippines     2373.
```

## 1.3

To filter gapminder to all rows that have a drop in life expectancy a new temporary variable of countrylag containing country values shifted one row down is made.
A new variable of increase in life expectancy is made from the difference in life expectancy between each year in order of country and year. Country and countrylag values are compared.  If they are not equal then its value is replaced with 0.  Increase in life expectancy is filtered for drop in life expectancy by filtering for negative values only. Countrylag is removed from the table.


```r
gapminder %>%
  mutate(countrylag = lag(country, order_by = country & year)) %>%
  mutate(increaseLifeExp = lag(diff(gapminder$lifeExp), order_by = country & year)) %>%
  mutate(increaseLifeExp = if_else( country == countrylag, increaseLifeExp, 0)) %>%
  filter(increaseLifeExp < 0) %>%
  select(-countrylag)
```

```
## # A tibble: 102 x 7
##    country  continent  year lifeExp     pop gdpPercap increaseLifeExp
##    <fct>    <fct>     <int>   <dbl>   <int>     <dbl>           <dbl>
##  1 Albania  Europe     1992    71.6 3326498     2497.          -0.419
##  2 Angola   Africa     1987    39.9 7874230     2430.          -0.036
##  3 Benin    Africa     2002    54.4 7026113     1373.          -0.371
##  4 Botswana Africa     1992    62.7 1342614     7954.          -0.877
##  5 Botswana Africa     1997    52.6 1536536     8647.         -10.2  
##  6 Botswana Africa     2002    46.6 1630347    11004.          -5.92 
##  7 Bulgaria Europe     1977    70.8 8797022     7612.          -0.09 
##  8 Bulgaria Europe     1992    71.2 8658506     6303.          -0.15 
##  9 Bulgaria Europe     1997    70.3 8066057     5970.          -0.87 
## 10 Burundi  Africa     1992    44.7 5809236      632.          -3.48 
## # … with 92 more rows
```


## 1.4

To filter gapminder to find the rows with the 3 largest and 3 smallest GDP per capita you can:

Find the number of rows in gapminder:

```r
nrow(gapminder)
```

```
## [1] 1704
```
 
Arrange gapminder in descending order for GDP per capita, then select top 3 rows and bottom 3 rows:


```r
(gapminder %>%
  arrange(desc(gdpPercap)))[c(1:3, 1702:1704), ]
```

```
## # A tibble: 6 x 6
##   country          continent  year lifeExp      pop gdpPercap
##   <fct>            <fct>     <int>   <dbl>    <int>     <dbl>
## 1 Kuwait           Asia       1957    58.0   212846   113523.
## 2 Kuwait           Asia       1972    67.7   841934   109348.
## 3 Kuwait           Asia       1952    55.6   160000   108382.
## 4 Lesotho          Africa     1952    42.1   748747      299.
## 5 Congo, Dem. Rep. Africa     2007    46.5 64606759      278.
## 6 Congo, Dem. Rep. Africa     2002    45.0 55379852      241.
```

## 1.5

Scatterplot of Canada's life expectancy vs. GDP per capita:


```r
ggplot(gapminder %>%
  filter( country == 'Canada'), aes(gdpPercap, lifeExp)) +
  geom_point(colour = 'blue') +
  scale_x_log10('log(gdpPercap)') 
```

![](hw02_dplyr_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

## 2

An exploration of continent and population variables of gapminder. 

Here is the data:



```r
gapminder %>%
          select(continent, pop)
```

```
## # A tibble: 1,704 x 2
##    continent      pop
##    <fct>        <int>
##  1 Asia       8425333
##  2 Asia       9240934
##  3 Asia      10267083
##  4 Asia      11537966
##  5 Asia      13079460
##  6 Asia      14880372
##  7 Asia      12881816
##  8 Asia      13867957
##  9 Asia      16317921
## 10 Asia      22227415
## # … with 1,694 more rows
```


Here is a summary of the data for continent and population variables:

```r
summary(gapminder %>%
          select(continent, pop))
```

```
##     continent        pop           
##  Africa  :624   Min.   :6.001e+04  
##  Americas:300   1st Qu.:2.794e+06  
##  Asia    :396   Median :7.024e+06  
##  Europe  :360   Mean   :2.960e+07  
##  Oceania : 24   3rd Qu.:1.959e+07  
##                 Max.   :1.319e+09
```

```r
1.319e+09 - 6.001e+04
```

```
## [1] 1318939990
```

The possible values for continent are Africa, Americas, Asia, Europe, and Oceania.  Population has a maximum value of 1.319e+09 and a minimum value of 6.001e+04 with a range of 1318939990. 

The following graph represents the population of each continent from 1951-2007:


```r
ggplot(gapminder, aes(continent, pop)) +
  geom_col() 
```

![](hw02_dplyr_files/figure-html/unnamed-chunk-11-1.png)<!-- -->
Asia has the largest population, while Oceania has the smallest population of the 5 categories.  Africa, Americas, and Europe have somewhat similar population levels relative to Asia and Oceania.


## 3


Exploration of Loblolly pine trees.  Here is a scatterplot of height (ft) vs. age (yr):


```r
ggplot(Loblolly, aes(age, height)) +
         geom_point(colour = 'magenta')
```

![](hw02_dplyr_files/figure-html/unnamed-chunk-12-1.png)<!-- -->

Here is a bar graph of height (ft) vs. seed source:


```r
ggplot(Loblolly, aes(Seed, height)) +
         geom_col()
```

![](hw02_dplyr_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

Seed type 305 appears to have the highest trees, while seed type 329 has the shortest.

## Recycling

No, the following code is incorrect to get data for Rwanda and Afghanistan:

```r
filter(gapminder, country == c("Rwanda", "Afghanistan"))
```

```
## # A tibble: 12 x 6
##    country     continent  year lifeExp      pop gdpPercap
##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
##  1 Afghanistan Asia       1957    30.3  9240934      821.
##  2 Afghanistan Asia       1967    34.0 11537966      836.
##  3 Afghanistan Asia       1977    38.4 14880372      786.
##  4 Afghanistan Asia       1987    40.8 13867957      852.
##  5 Afghanistan Asia       1997    41.8 22227415      635.
##  6 Afghanistan Asia       2007    43.8 31889923      975.
##  7 Rwanda      Africa     1952    40    2534927      493.
##  8 Rwanda      Africa     1962    43    3051242      597.
##  9 Rwanda      Africa     1972    44.6  3992121      591.
## 10 Rwanda      Africa     1982    46.2  5507565      882.
## 11 Rwanda      Africa     1992    23.6  7290203      737.
## 12 Rwanda      Africa     2002    43.4  7852401      786.
```
 
Because creating multiple entries means both values will not be check for the condition to be true. This is the correct way to check for both:

```r
filter(gapminder, country == "Rwanda" | country == "Afghanistan")
```

```
## # A tibble: 24 x 6
##    country     continent  year lifeExp      pop gdpPercap
##    <fct>       <fct>     <int>   <dbl>    <int>     <dbl>
##  1 Afghanistan Asia       1952    28.8  8425333      779.
##  2 Afghanistan Asia       1957    30.3  9240934      821.
##  3 Afghanistan Asia       1962    32.0 10267083      853.
##  4 Afghanistan Asia       1967    34.0 11537966      836.
##  5 Afghanistan Asia       1972    36.1 13079460      740.
##  6 Afghanistan Asia       1977    38.4 14880372      786.
##  7 Afghanistan Asia       1982    39.9 12881816      978.
##  8 Afghanistan Asia       1987    40.8 13867957      852.
##  9 Afghanistan Asia       1992    41.7 16317921      649.
## 10 Afghanistan Asia       1997    41.8 22227415      635.
## # … with 14 more rows
```

