---
title: "hw04-exercises"
output: 
  html_document:
    keep_md: true
    theme: paper
---

loads packages:

```r
library(tidyverse)
library(gapminder)
```

# Exercise 1: Univariate Option 2

Lets calculate the mean life expectancy for all combinations of continent and year from the gapminder dataset.  This is dont first by grouping continent and year together then using summarize to add a new column with the mean life expectancies (meanLifeExp). This will then be reshaped using pivot_wider to make one row per year and a new column for each continent with values for each of their mean life expectancies.

```r
gapminder_wider <-gapminder %>%
  group_by(continent, year) %>% 
  summarize(meanLifeExp = mean(lifeExp)) %>%
  pivot_wider(id_cols = year,
              names_from = continent,
              values_from = meanLifeExp) 
DT::datatable(gapminder_wider)
```

<!--html_preserve--><div id="htmlwidget-60e023ae4dc8632a432b" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-60e023ae4dc8632a432b">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8","9","10","11","12"],[1952,1957,1962,1967,1972,1977,1982,1987,1992,1997,2002,2007],[39.1355,41.2663461538462,43.3194423076923,45.3345384615385,47.4509423076923,49.5804230769231,51.5928653846154,53.3447884615385,53.6295769230769,53.5982692307692,53.3252307692308,54.8060384615385],[53.27984,55.96028,58.39876,60.41092,62.39492,64.39156,66.22884,68.09072,69.56836,71.15048,72.42204,73.60812],[46.3143939393939,49.3185442424242,51.563223030303,54.66364,57.3192690909091,59.6105563636364,62.6179393939394,64.8511818181818,66.5372121212121,68.0205151515152,69.2338787878788,70.7284848484849],[64.4085,66.7030666666667,68.5392333333333,69.7376,70.7750333333333,71.9377666666667,72.8064,73.6421666666667,74.4401,75.5051666666667,76.7006,77.6486],[69.255,70.295,71.085,71.31,71.91,72.855,74.29,75.32,76.945,78.19,79.74,80.7195]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> <\/th>\n      <th>year<\/th>\n      <th>Africa<\/th>\n      <th>Americas<\/th>\n      <th>Asia<\/th>\n      <th>Europe<\/th>\n      <th>Oceania<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"columnDefs":[{"className":"dt-right","targets":[1,2,3,4,5,6]},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

Lets plot the mean life expectancy of only the continent Africa over time:

```r
ggplot(gapminder_wider, aes(year, Africa)) +
  geom_point(colour = "cyan", size = 3) +
  theme_dark() +
  ylab("mean life expectancy") +
  ggtitle("Africa's mean life expectancy over time") +
  geom_smooth(method = "lm", colour = 'magenta') 
```

![](hw04-exercises_files/figure-html/unnamed-chunk-3-1.png)<!-- -->


Now we will reshape the data back to its longer format with pivot_longer:

```r
gapminder_wider %>%
  pivot_longer(cols = c(Africa, Americas, Asia, Europe, Oceania),
               names_to = "continent",
               values_to = "meanLifeExp") %>%
  DT::datatable()
```

<!--html_preserve--><div id="htmlwidget-674999c12606a6b355c1" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-674999c12606a6b355c1">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30","31","32","33","34","35","36","37","38","39","40","41","42","43","44","45","46","47","48","49","50","51","52","53","54","55","56","57","58","59","60"],[1952,1952,1952,1952,1952,1957,1957,1957,1957,1957,1962,1962,1962,1962,1962,1967,1967,1967,1967,1967,1972,1972,1972,1972,1972,1977,1977,1977,1977,1977,1982,1982,1982,1982,1982,1987,1987,1987,1987,1987,1992,1992,1992,1992,1992,1997,1997,1997,1997,1997,2002,2002,2002,2002,2002,2007,2007,2007,2007,2007],["Africa","Americas","Asia","Europe","Oceania","Africa","Americas","Asia","Europe","Oceania","Africa","Americas","Asia","Europe","Oceania","Africa","Americas","Asia","Europe","Oceania","Africa","Americas","Asia","Europe","Oceania","Africa","Americas","Asia","Europe","Oceania","Africa","Americas","Asia","Europe","Oceania","Africa","Americas","Asia","Europe","Oceania","Africa","Americas","Asia","Europe","Oceania","Africa","Americas","Asia","Europe","Oceania","Africa","Americas","Asia","Europe","Oceania","Africa","Americas","Asia","Europe","Oceania"],[39.1355,53.27984,46.3143939393939,64.4085,69.255,41.2663461538462,55.96028,49.3185442424242,66.7030666666667,70.295,43.3194423076923,58.39876,51.563223030303,68.5392333333333,71.085,45.3345384615385,60.41092,54.66364,69.7376,71.31,47.4509423076923,62.39492,57.3192690909091,70.7750333333333,71.91,49.5804230769231,64.39156,59.6105563636364,71.9377666666667,72.855,51.5928653846154,66.22884,62.6179393939394,72.8064,74.29,53.3447884615385,68.09072,64.8511818181818,73.6421666666667,75.32,53.6295769230769,69.56836,66.5372121212121,74.4401,76.945,53.5982692307692,71.15048,68.0205151515152,75.5051666666667,78.19,53.3252307692308,72.42204,69.2338787878788,76.7006,79.74,54.8060384615385,73.60812,70.7284848484849,77.6486,80.7195]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> <\/th>\n      <th>year<\/th>\n      <th>continent<\/th>\n      <th>meanLifeExp<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"columnDefs":[{"className":"dt-right","targets":[1,3]},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

# Exercise 2: Multivariate Option 1

Lets filter gapminder for only the 2 countries Canada and Spain and select all variables except continent and population.  Next, we will reshape using pivot_wider to make a tibble with one row per year and columns for life expectancy and GDP per capita.

```r
gapminder_wider2 <-gapminder %>%
  filter(country == "Canada" | country == "Spain") %>%
  select(-continent, -pop) %>%
  pivot_wider(id_cols = year,
              names_from = country,
              names_sep = "_",
              values_from = c(lifeExp, gdpPercap))
DT::datatable(gapminder_wider2)
```

<!--html_preserve--><div id="htmlwidget-b00e9bc3f7bf7e0f11de" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-b00e9bc3f7bf7e0f11de">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8","9","10","11","12"],[1952,1957,1962,1967,1972,1977,1982,1987,1992,1997,2002,2007],[68.75,69.96,71.3,72.13,72.88,74.21,75.76,76.86,77.95,78.61,79.77,80.653],[64.94,66.66,69.69,71.44,73.06,74.39,76.3,76.9,77.57,78.77,79.78,80.941],[11367.16112,12489.95006,13462.48555,16076.58803,18970.57086,22090.88306,22898.79214,26626.51503,26342.88426,28954.92589,33328.96507,36319.23501],[3834.034742,4564.80241,5693.843879,7993.512294,10638.75131,13236.92117,13926.16997,15764.98313,18603.06452,20445.29896,24835.47166,28821.0637]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> <\/th>\n      <th>year<\/th>\n      <th>lifeExp_Canada<\/th>\n      <th>lifeExp_Spain<\/th>\n      <th>gdpPercap_Canada<\/th>\n      <th>gdpPercap_Spain<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"columnDefs":[{"className":"dt-right","targets":[1,2,3,4,5]},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

Now we will reshape the data back to its longer format with pivot_longer:

```r
gapminder_wider2 %>%
  pivot_longer(cols = -year,
               names_to = c(".value", "country"),
               names_sep = "_") %>%
  DT::datatable()
```

<!--html_preserve--><div id="htmlwidget-50d58fa8e3f649c6fc5e" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-50d58fa8e3f649c6fc5e">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24"],[1952,1952,1957,1957,1962,1962,1967,1967,1972,1972,1977,1977,1982,1982,1987,1987,1992,1992,1997,1997,2002,2002,2007,2007],["Canada","Spain","Canada","Spain","Canada","Spain","Canada","Spain","Canada","Spain","Canada","Spain","Canada","Spain","Canada","Spain","Canada","Spain","Canada","Spain","Canada","Spain","Canada","Spain"],[68.75,64.94,69.96,66.66,71.3,69.69,72.13,71.44,72.88,73.06,74.21,74.39,75.76,76.3,76.86,76.9,77.95,77.57,78.61,78.77,79.77,79.78,80.653,80.941],[11367.16112,3834.034742,12489.95006,4564.80241,13462.48555,5693.843879,16076.58803,7993.512294,18970.57086,10638.75131,22090.88306,13236.92117,22898.79214,13926.16997,26626.51503,15764.98313,26342.88426,18603.06452,28954.92589,20445.29896,33328.96507,24835.47166,36319.23501,28821.0637]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> <\/th>\n      <th>year<\/th>\n      <th>country<\/th>\n      <th>lifeExp<\/th>\n      <th>gdpPercap<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"columnDefs":[{"className":"dt-right","targets":[1,3,4]},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

# Exercise 3: Table Joins

load datasets:

```r
guest <- read_csv("https://raw.githubusercontent.com/STAT545-UBC/Classroom/master/data/wedding/attend.csv")
email <- read_csv("https://raw.githubusercontent.com/STAT545-UBC/Classroom/master/data/wedding/emails.csv")
```


```r
guest
```

```
## # A tibble: 30 x 7
##    party name  meal_wedding meal_brunch attendance_wedd… attendance_brun…
##    <dbl> <chr> <chr>        <chr>       <chr>            <chr>           
##  1     1 Somm… PENDING      PENDING     PENDING          PENDING         
##  2     1 Phil… vegetarian   Menu C      CONFIRMED        CONFIRMED       
##  3     1 Blan… chicken      Menu A      CONFIRMED        CONFIRMED       
##  4     1 Emaa… PENDING      PENDING     PENDING          PENDING         
##  5     2 Blai… chicken      Menu C      CONFIRMED        CONFIRMED       
##  6     2 Nige… <NA>         <NA>        CANCELLED        CANCELLED       
##  7     3 Sine… PENDING      PENDING     PENDING          PENDING         
##  8     4 Ayra… vegetarian   Menu B      PENDING          PENDING         
##  9     5 Atla… PENDING      PENDING     PENDING          PENDING         
## 10     5 Denz… fish         Menu B      CONFIRMED        CONFIRMED       
## # … with 20 more rows, and 1 more variable: attendance_golf <chr>
```


```r
email <- email %>%
  rename(name = guest) %>%
  separate_rows(name, sep = ", ")
email
```

```
## # A tibble: 28 x 2
##    name            email              
##    <chr>           <chr>              
##  1 Sommer Medrano  sommm@gmail.com    
##  2 Phillip Medrano sommm@gmail.com    
##  3 Blanka Medrano  sommm@gmail.com    
##  4 Emaan Medrano   sommm@gmail.com    
##  5 Blair Park      bpark@gmail.com    
##  6 Nigel Webb      bpark@gmail.com    
##  7 Sinead English  singlish@hotmail.ca
##  8 Ayra Marks      marksa42@gmail.com 
##  9 Jolene Welsh    jw1987@hotmail.com 
## 10 Hayley Booker   jw1987@hotmail.com 
## # … with 18 more rows
```

## 3.1

For each guest in the guestlist (guest tibble), add a column for email address, which can be found in the email tibble.


```r
guest %>%
  left_join(email, by = "name") %>%
  DT::datatable()
```

<!--html_preserve--><div id="htmlwidget-84c34b31eacf8bcc024a" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-84c34b31eacf8bcc024a">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30"],[1,1,1,1,2,2,3,4,5,5,5,6,6,7,7,8,9,10,11,12,12,12,12,12,13,13,14,14,15,15],["Sommer Medrano","Phillip Medrano","Blanka Medrano","Emaan Medrano","Blair Park","Nigel Webb","Sinead English","Ayra Marks","Atlanta Connolly","Denzel Connolly","Chanelle Shah","Jolene Welsh","Hayley Booker","Amayah Sanford","Erika Foley","Ciaron Acosta","Diana Stuart","Cosmo Dunkley","Cai Mcdaniel","Daisy-May Caldwell","Martin Caldwell","Violet Caldwell","Nazifa Caldwell","Eric Caldwell","Rosanna Bird","Kurtis Frost","Huma Stokes","Samuel Rutledge","Eddison Collier","Stewart Nicholls"],["PENDING","vegetarian","chicken","PENDING","chicken",null,"PENDING","vegetarian","PENDING","fish","chicken",null,"vegetarian",null,"PENDING","PENDING","vegetarian","PENDING","fish","chicken","PENDING","PENDING","chicken","chicken","vegetarian","PENDING",null,"chicken","PENDING","chicken"],["PENDING","Menu C","Menu A","PENDING","Menu C",null,"PENDING","Menu B","PENDING","Menu B","Menu C",null,"Menu C","PENDING","PENDING","Menu A","Menu C","PENDING","Menu C","Menu B","PENDING","PENDING","PENDING","Menu B","Menu C","PENDING",null,"Menu C","PENDING","Menu B"],["PENDING","CONFIRMED","CONFIRMED","PENDING","CONFIRMED","CANCELLED","PENDING","PENDING","PENDING","CONFIRMED","CONFIRMED","CANCELLED","CONFIRMED","CANCELLED","PENDING","PENDING","CONFIRMED","PENDING","CONFIRMED","CONFIRMED","PENDING","PENDING","PENDING","CONFIRMED","CONFIRMED","PENDING","CANCELLED","CONFIRMED","PENDING","CONFIRMED"],["PENDING","CONFIRMED","CONFIRMED","PENDING","CONFIRMED","CANCELLED","PENDING","PENDING","PENDING","CONFIRMED","CONFIRMED","CANCELLED","CONFIRMED","PENDING","PENDING","PENDING","CONFIRMED","PENDING","CONFIRMED","CONFIRMED","PENDING","PENDING","PENDING","CONFIRMED","CONFIRMED","PENDING","CANCELLED","CONFIRMED","PENDING","CONFIRMED"],["PENDING","CONFIRMED","CONFIRMED","PENDING","CONFIRMED","CANCELLED","PENDING","PENDING","PENDING","CONFIRMED","CONFIRMED","CANCELLED","CONFIRMED","PENDING","PENDING","PENDING","CONFIRMED","PENDING","CONFIRMED","CONFIRMED","PENDING","PENDING","PENDING","CONFIRMED","CONFIRMED","PENDING","CANCELLED","CONFIRMED","PENDING","CONFIRMED"],["sommm@gmail.com","sommm@gmail.com","sommm@gmail.com","sommm@gmail.com","bpark@gmail.com","bpark@gmail.com","singlish@hotmail.ca","marksa42@gmail.com",null,null,null,"jw1987@hotmail.com","jw1987@hotmail.com","erikaaaaaa@gmail.com","erikaaaaaa@gmail.com","shining_ciaron@gmail.com","doodledianastu@gmail.com",null,null,"caldwellfamily5212@gmail.com","caldwellfamily5212@gmail.com","caldwellfamily5212@gmail.com","caldwellfamily5212@gmail.com","caldwellfamily5212@gmail.com","rosy1987b@gmail.com","rosy1987b@gmail.com","humastokes@gmail.com","humastokes@gmail.com","eddison.collier@gmail.com","eddison.collier@gmail.com"]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> <\/th>\n      <th>party<\/th>\n      <th>name<\/th>\n      <th>meal_wedding<\/th>\n      <th>meal_brunch<\/th>\n      <th>attendance_wedding<\/th>\n      <th>attendance_brunch<\/th>\n      <th>attendance_golf<\/th>\n      <th>email<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"columnDefs":[{"className":"dt-right","targets":1},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

## 3.2

Who do we have emails for, yet are not on the guestlist?


```r
setdiff(email$name, guest$name)
```

```
## [1] "Turner Jones"    "Albert Marshall" "Vivian Marshall"
```

## 3.3

Make a guestlist that includes everyone we have emails for (in addition to those on the original guestlist).


```r
guest %>%
  full_join(email, by = "name") %>%
  DT::datatable()
```

<!--html_preserve--><div id="htmlwidget-7d264d3482ba1ea530e6" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-7d264d3482ba1ea530e6">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30","31","32","33"],[1,1,1,1,2,2,3,4,5,5,5,6,6,7,7,8,9,10,11,12,12,12,12,12,13,13,14,14,15,15,null,null,null],["Sommer Medrano","Phillip Medrano","Blanka Medrano","Emaan Medrano","Blair Park","Nigel Webb","Sinead English","Ayra Marks","Atlanta Connolly","Denzel Connolly","Chanelle Shah","Jolene Welsh","Hayley Booker","Amayah Sanford","Erika Foley","Ciaron Acosta","Diana Stuart","Cosmo Dunkley","Cai Mcdaniel","Daisy-May Caldwell","Martin Caldwell","Violet Caldwell","Nazifa Caldwell","Eric Caldwell","Rosanna Bird","Kurtis Frost","Huma Stokes","Samuel Rutledge","Eddison Collier","Stewart Nicholls","Turner Jones","Albert Marshall","Vivian Marshall"],["PENDING","vegetarian","chicken","PENDING","chicken",null,"PENDING","vegetarian","PENDING","fish","chicken",null,"vegetarian",null,"PENDING","PENDING","vegetarian","PENDING","fish","chicken","PENDING","PENDING","chicken","chicken","vegetarian","PENDING",null,"chicken","PENDING","chicken",null,null,null],["PENDING","Menu C","Menu A","PENDING","Menu C",null,"PENDING","Menu B","PENDING","Menu B","Menu C",null,"Menu C","PENDING","PENDING","Menu A","Menu C","PENDING","Menu C","Menu B","PENDING","PENDING","PENDING","Menu B","Menu C","PENDING",null,"Menu C","PENDING","Menu B",null,null,null],["PENDING","CONFIRMED","CONFIRMED","PENDING","CONFIRMED","CANCELLED","PENDING","PENDING","PENDING","CONFIRMED","CONFIRMED","CANCELLED","CONFIRMED","CANCELLED","PENDING","PENDING","CONFIRMED","PENDING","CONFIRMED","CONFIRMED","PENDING","PENDING","PENDING","CONFIRMED","CONFIRMED","PENDING","CANCELLED","CONFIRMED","PENDING","CONFIRMED",null,null,null],["PENDING","CONFIRMED","CONFIRMED","PENDING","CONFIRMED","CANCELLED","PENDING","PENDING","PENDING","CONFIRMED","CONFIRMED","CANCELLED","CONFIRMED","PENDING","PENDING","PENDING","CONFIRMED","PENDING","CONFIRMED","CONFIRMED","PENDING","PENDING","PENDING","CONFIRMED","CONFIRMED","PENDING","CANCELLED","CONFIRMED","PENDING","CONFIRMED",null,null,null],["PENDING","CONFIRMED","CONFIRMED","PENDING","CONFIRMED","CANCELLED","PENDING","PENDING","PENDING","CONFIRMED","CONFIRMED","CANCELLED","CONFIRMED","PENDING","PENDING","PENDING","CONFIRMED","PENDING","CONFIRMED","CONFIRMED","PENDING","PENDING","PENDING","CONFIRMED","CONFIRMED","PENDING","CANCELLED","CONFIRMED","PENDING","CONFIRMED",null,null,null],["sommm@gmail.com","sommm@gmail.com","sommm@gmail.com","sommm@gmail.com","bpark@gmail.com","bpark@gmail.com","singlish@hotmail.ca","marksa42@gmail.com",null,null,null,"jw1987@hotmail.com","jw1987@hotmail.com","erikaaaaaa@gmail.com","erikaaaaaa@gmail.com","shining_ciaron@gmail.com","doodledianastu@gmail.com",null,null,"caldwellfamily5212@gmail.com","caldwellfamily5212@gmail.com","caldwellfamily5212@gmail.com","caldwellfamily5212@gmail.com","caldwellfamily5212@gmail.com","rosy1987b@gmail.com","rosy1987b@gmail.com","humastokes@gmail.com","humastokes@gmail.com","eddison.collier@gmail.com","eddison.collier@gmail.com","tjjones12@hotmail.ca","themarshallfamily1234@gmail.com","themarshallfamily1234@gmail.com"]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> <\/th>\n      <th>party<\/th>\n      <th>name<\/th>\n      <th>meal_wedding<\/th>\n      <th>meal_brunch<\/th>\n      <th>attendance_wedding<\/th>\n      <th>attendance_brunch<\/th>\n      <th>attendance_golf<\/th>\n      <th>email<\/th>\n    <\/tr>\n  <\/thead>\n<\/table>","options":{"columnDefs":[{"className":"dt-right","targets":1},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

