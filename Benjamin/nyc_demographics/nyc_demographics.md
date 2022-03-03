---
title: "Making NYC Demographics Data Frames"
author: "Benjamin Prieto"
date: '2022-03-03'
output: 
  html_document: 
    keep_md: yes
---

### Setup

[Data Source](https://www1.nyc.gov/site/planning/planning-level/nyc-population/american-community-survey.page.page) 

```r
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
```

```
## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
## ✓ tibble  3.1.6     ✓ dplyr   1.0.8
## ✓ tidyr   1.2.0     ✓ stringr 1.4.0
## ✓ readr   2.1.2     ✓ forcats 0.5.1
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(janitor)
```

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

### 2015 Data

```r
demo_2015_raw <- read.csv("demo_2015acs1yr_nyc.csv")
```


```r
demo_2015_age <- demo_2015_raw %>% 
  select(DP05..ACS.DEMOGRAPHIC.AND.HOUSING.ESTIMATES:X.3) %>% 
  slice(12:24)
colnames(demo_2015_age) <- c("age_group","population","margin_error","percent_pop_for_year","percent_pop_for_year_margin_error")
demo_2015_age$population <- str_remove(string = demo_2015_age$population, pattern = ",")
demo_2015_age$population <- str_remove(string = demo_2015_age$population, pattern = ",")
demo_2015_age$population <- as.numeric(demo_2015_age$population)
demo_2015_age <- demo_2015_age %>% 
  select(age_group, population) %>% 
  mutate(percent_pop_for_year = 100*population/sum(population), year = 2015)
demo_2015_age
```

```
##                 age_group population percent_pop_for_year year
## 1         Under 5 years       569712             6.662983 2015
## 2          5 to 9 years       482350             5.641253 2015
## 3        10 to 14 years       471932             5.519411 2015
## 4        15 to 19 years       469766             5.494079 2015
## 5        20 to 24 years       608130             7.112295 2015
## 6        25 to 34 years      1526724            17.855575 2015
## 7        35 to 44 years      1186890            13.881097 2015
## 8        45 to 54 years      1118835            13.085170 2015
## 9        55 to 59 years       523518             6.122728 2015
## 10       60 to 64 years       465037             5.438772 2015
## 11       65 to 74 years       635453             7.431847 2015
## 12       75 to 84 years       337914             3.952023 2015
## 13    85 years and over       154144             1.802768 2015
```


```r
demo_2015_race <- demo_2015_raw %>% 
  select(DP05..ACS.DEMOGRAPHIC.AND.HOUSING.ESTIMATES:X.3) %>% 
  slice(c(47:49, 54, 62, 67, 68))
colnames(demo_2015_race) <- c("race","population","margin_error","percent_pop_for_year","percent_pop_for_year_margin_error")
demo_2015_race$population <- str_remove(string = demo_2015_race$population, pattern = ",")
demo_2015_race$population <- str_remove(string = demo_2015_race$population, pattern = ",")
demo_2015_race$population <- as.numeric(demo_2015_race$population)
demo_2015_race <- demo_2015_race %>% 
  select(race, population) %>% 
  mutate(percent_pop_for_year = 100*population/sum(population), year = 2015)
demo_2015_race
```

```
##                                                race population
## 1                                           White      3639914
## 2                       Black or African American      2055137
## 3               American Indian and Alaska Native        35617
## 4                                           Asian      1205321
## 5      Native Hawaiian and Other Pacific Islander         1864
## 6                                 Some other race      1328369
## 7                               Two or more races       284183
##   percent_pop_for_year year
## 1          42.57007709 2015
## 2          24.03555153 2015
## 3           0.41655337 2015
## 4          14.09665390 2015
## 5           0.02180014 2015
## 6          15.53574363 2015
## 7           3.32362034 2015
```

### 2014 Data

```r
demo_2014_raw <- read.csv("demo_2014acs1yr_nyc.csv")
```


```r
demo_2014_age <- demo_2014_raw %>% 
  select(DP05..ACS.DEMOGRAPHIC.AND.HOUSING.ESTIMATES:X.3) %>% 
  slice(12:24)
colnames(demo_2014_age) <- c("age_group","population","margin_error","percent_pop_for_year","percent_pop_for_year_margin_error")
demo_2014_age$population <- str_remove(string = demo_2014_age$population, pattern = ",")
demo_2014_age$population <- str_remove(string = demo_2014_age$population, pattern = ",")
demo_2014_age$population <- as.numeric(demo_2014_age$population)
demo_2014_age <- demo_2014_age %>% 
  select(age_group, population) %>% 
  mutate(percent_pop_for_year = 100*population/sum(population), year = 2014)
demo_2014_age
```

```
##                 age_group population percent_pop_for_year year
## 1         Under 5 years       567142             6.679269 2014
## 2          5 to 9 years       492017             5.794517 2014
## 3        10 to 14 years       456957             5.381613 2014
## 4        15 to 19 years       473714             5.578961 2014
## 5        20 to 24 years       626933             7.383431 2014
## 6        25 to 34 years      1504601            17.719786 2014
## 7        35 to 44 years      1181189            13.910941 2014
## 8        45 to 54 years      1117471            13.160530 2014
## 9        55 to 59 years       521993             6.147546 2014
## 10       60 to 64 years       450669             5.307559 2014
## 11       65 to 74 years       612532             7.213830 2014
## 12       75 to 84 years       328189             3.865104 2014
## 13    85 years and over       157672             1.856914 2014
```


```r
demo_2014_race <- demo_2014_raw %>% 
  select(DP05..ACS.DEMOGRAPHIC.AND.HOUSING.ESTIMATES:X.3) %>% 
  slice(c(47:49, 54, 62, 67, 68))
colnames(demo_2014_race) <- c("race","population","margin_error","percent_pop_for_year","percent_pop_for_year_margin_error")
demo_2014_race$population <- str_remove(string = demo_2014_race$population, pattern = ",")
demo_2014_race$population <- str_remove(string = demo_2014_race$population, pattern = ",")
demo_2014_race$population <- as.numeric(demo_2014_race$population)
demo_2014_race <- demo_2014_race %>% 
  select(race, population) %>% 
  mutate(percent_pop_for_year = 100*population/sum(population), year = 2014)
demo_2014_race
```

```
##                                                race population
## 1                                           White      3619690
## 2                       Black or African American      2079129
## 3               American Indian and Alaska Native        33983
## 4                                           Asian      1172114
## 5      Native Hawaiian and Other Pacific Islander         2946
## 6                                 Some other race      1317266
## 7                               Two or more races       265951
##   percent_pop_for_year year
## 1          42.62932897 2014
## 2          24.48604000 2014
## 3           0.40022004 2014
## 4          13.80406424 2014
## 5           0.03469524 2014
## 6          15.51352896 2014
## 7           3.13212255 2014
```

### 2013 Data

```r
demo_2013_raw <- read.csv("boro_demo_2013_acs.csv")
```


```r
demo_2013_age <- demo_2013_raw %>% 
  select(DP05..ACS.DEMOGRAPHIC.AND.HOUSING.ESTIMATES:X.3) %>% 
  slice(12:24)
colnames(demo_2013_age) <- c("age_group","population","margin_error","percent_pop_for_year","percent_pop_for_year_margin_error")
demo_2013_age$population <- str_remove(string = demo_2013_age$population, pattern = ",")
demo_2013_age$population <- str_remove(string = demo_2013_age$population, pattern = ",")
demo_2013_age$population <- as.numeric(demo_2013_age$population)
demo_2013_age <- demo_2013_age %>% 
  select(age_group, population) %>% 
  mutate(percent_pop_for_year = 100*population/sum(population), year = 2013)
demo_2013_age
```

```
##                 age_group population percent_pop_for_year year
## 1         Under 5 years       555756             6.611549 2013
## 2          5 to 9 years       479102             5.699635 2013
## 3        10 to 14 years       466303             5.547371 2013
## 4        15 to 19 years       481367             5.726580 2013
## 5        20 to 24 years       634623             7.549790 2013
## 6        25 to 34 years      1478414            17.587945 2013
## 7        35 to 44 years      1170936            13.930035 2013
## 8        45 to 54 years      1112156            13.230759 2013
## 9        55 to 59 years       505267             6.010906 2013
## 10       60 to 64 years       447366             5.322087 2013
## 11       65 to 74 years       593249             7.057584 2013
## 12       75 to 84 years       325287             3.869775 2013
## 13    85 years and over       156011             1.855984 2013
```


```r
demo_2013_race <- demo_2013_raw %>% 
  select(DP05..ACS.DEMOGRAPHIC.AND.HOUSING.ESTIMATES:X.3) %>% 
  slice(c(47:49, 54, 62, 67, 68))
colnames(demo_2013_race) <- c("race","population","margin_error","percent_pop_for_year","percent_pop_for_year_margin_error")
demo_2013_race$population <- str_remove(string = demo_2013_race$population, pattern = ",")
demo_2013_race$population <- str_remove(string = demo_2013_race$population, pattern = ",")
demo_2013_race$population <- as.numeric(demo_2013_race$population)
demo_2013_race <- demo_2013_race %>% 
  select(race, population) %>% 
  mutate(percent_pop_for_year = 100*population/sum(population), year = 2013)
demo_2013_race
```

```
##                                                race population
## 1                                           White      3659632
## 2                       Black or African American      2070401
## 3               American Indian and Alaska Native        30380
## 4                                           Asian      1133042
## 5      Native Hawaiian and Other Pacific Islander         5908
## 6                                 Some other race      1249233
## 7                               Two or more races       257241
##   percent_pop_for_year year
## 1          43.53679473 2013
## 2          24.63051568 2013
## 3           0.36141553 2013
## 4          13.47922878 2013
## 5           0.07028449 2013
## 6          14.86149446 2013
## 7           3.06026634 2013
```

### 2012 Data

```r
demo_2012_raw <- read.csv("boro_demo_2012_acs.csv")
```


```r
demo_2012_age <- demo_2012_raw %>% 
  select(DP05..ACS.DEMOGRAPHIC.AND.HOUSING.ESTIMATES:X.3) %>% 
  slice(11:23)
colnames(demo_2012_age) <- c("age_group","population","margin_error","percent_pop_for_year","percent_pop_for_year_margin_error")
demo_2012_age$population <- str_remove(string = demo_2012_age$population, pattern = ",")
demo_2012_age$population <- str_remove(string = demo_2012_age$population, pattern = ",")
demo_2012_age$population <- as.numeric(demo_2012_age$population)
demo_2012_age <- demo_2012_age %>% 
  select(age_group, population) %>% 
  mutate(percent_pop_for_year = 100*population/sum(population), year = 2012)
demo_2012_age
```

```
##              age_group population percent_pop_for_year year
## 1        Under 5 years     544369             6.529792 2012
## 2         5 to 9 years     476525             5.715993 2012
## 3       10 to 14 years     471011             5.649851 2012
## 4       15 to 19 years     498696             5.981937 2012
## 5       20 to 24 years     643802             7.722507 2012
## 6       25 to 34 years    1450056            17.393651 2012
## 7       35 to 44 years    1161456            13.931849 2012
## 8       45 to 54 years    1108238            13.293490 2012
## 9       55 to 59 years     493406             5.918483 2012
## 10      60 to 64 years     443050             5.314455 2012
## 11      65 to 74 years     572984             6.873034 2012
## 12      75 to 84 years     320241             3.841341 2012
## 13   85 years and over     152863             1.833616 2012
```


```r
demo_2012_race <- demo_2012_raw %>% 
  select(DP05..ACS.DEMOGRAPHIC.AND.HOUSING.ESTIMATES:X.3) %>% 
  slice(c(46:48, 53, 61, 66, 67))
colnames(demo_2012_race) <- c("race","population","margin_error","percent_pop_for_year","percent_pop_for_year_margin_error")
demo_2012_race$population <- str_remove(string = demo_2012_race$population, pattern = ",")
demo_2012_race$population <- str_remove(string = demo_2012_race$population, pattern = ",")
demo_2012_race$population <- as.numeric(demo_2012_race$population)
demo_2012_race <- demo_2012_race %>% 
  select(race, population) %>% 
  mutate(percent_pop_for_year = 100*population/sum(population), year = 2012)
demo_2012_race
```

```
##                                             race population
## 1                                          White    3647131
## 2                      Black or African American    2058522
## 3              American Indian and Alaska Native      30230
## 4                                          Asian    1091449
## 5     Native Hawaiian and Other Pacific Islander       3548
## 6                                Some other race    1251568
## 7                              Two or more races     254249
##   percent_pop_for_year year
## 1          43.74791359 2012
## 2          24.69229720 2012
## 3           0.36261363 2012
## 4          13.09210350 2012
## 5           0.04255882 2012
## 6          15.01275625 2012
## 7           3.04975700 2012
```

### 2011 Data

```r
demo_2011_raw <- read_csv("boro_demo_2011_acs.csv") %>% 
  clean_names()
```

```
## New names:
## * `` -> ...2
## * `` -> ...3
## * `` -> ...4
## * `` -> ...5
## * `` -> ...6
## * ...
```

```
## Rows: 136 Columns: 25
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (25): DP05: ACS DEMOGRAPHIC AND HOUSING ESTIMATES, ...2, ...3, ...4, ......
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```


```r
demo_2011_age <- demo_2011_raw %>% 
  select(dp05_acs_demographic_and_housing_estimates:x3) %>% 
  slice(11:23)
colnames(demo_2011_age) <- c("age_group","population","margin_error","percent_pop_for_year","percent_pop_for_year_margin_error")
demo_2011_age$population <- str_remove(string = demo_2011_age$population, pattern = ",")
demo_2011_age$population <- str_remove(string = demo_2011_age$population, pattern = ",")
demo_2011_age$population <- as.numeric(demo_2011_age$population)
demo_2011_age <- demo_2011_age %>% 
  select(age_group, population) %>% 
  mutate(percent_pop_for_year = 100*population/sum(population), year = 2011)
demo_2011_age
```

```
## # A tibble: 13 × 4
##    age_group         population percent_pop_for_year  year
##    <chr>                  <dbl>                <dbl> <dbl>
##  1 Under 5 years         534400                 6.48  2011
##  2 5 to 9 years          473915                 5.75  2011
##  3 10 to 14 years        467219                 5.67  2011
##  4 15 to 19 years        508178                 6.16  2011
##  5 20 to 24 years        643544                 7.81  2011
##  6 25 to 34 years       1423451                17.3   2011
##  7 35 to 44 years       1151332                14.0   2011
##  8 45 to 54 years       1109379                13.5   2011
##  9 55 to 59 years        495114                 6.01  2011
## 10 60 to 64 years        426382                 5.17  2011
## 11 65 to 74 years        544766                 6.61  2011
## 12 75 to 84 years        323957                 3.93  2011
## 13 85 years and over     143273                 1.74  2011
```


```r
demo_2011_race <- demo_2011_raw %>% 
  select(dp05_acs_demographic_and_housing_estimates:x3) %>% 
  slice(c(40:42, 47, 55, 60, 61))
colnames(demo_2011_race) <- c("race","population","margin_error","percent_pop_for_year","percent_pop_for_year_margin_error")
demo_2011_race$population <- str_remove(string = demo_2011_race$population, pattern = ",")
demo_2011_race$population <- str_remove(string = demo_2011_race$population, pattern = ",")
demo_2011_race$population <- as.numeric(demo_2011_race$population)
demo_2011_race <- demo_2011_race %>% 
  select(race, population) %>% 
  mutate(percent_pop_for_year = 100*population/sum(population), year = 2011)
demo_2011_race
```

```
## # A tibble: 7 × 4
##   race                                       population percent_pop_for_y…  year
##   <chr>                                           <dbl>              <dbl> <dbl>
## 1 White                                         3618421            43.9     2011
## 2 Black or African American                     2054101            24.9     2011
## 3 American Indian and Alaska Native               27390             0.332   2011
## 4 Asian                                         1051946            12.8     2011
## 5 Native Hawaiian and Other Pacific Islander       4321             0.0524  2011
## 6 Some other race                               1236450            15.0     2011
## 7 Two or more races                              252281             3.06    2011
```
### Join Data Frames


```r
demo_age_list <- list(demo_2011_age, demo_2012_age, demo_2013_age, demo_2014_age, demo_2015_age)
demo_age_all <- bind_rows(demo_age_list)
demo_age_all
```

```
## # A tibble: 65 × 4
##    age_group      population percent_pop_for_year  year
##    <chr>               <dbl>                <dbl> <dbl>
##  1 Under 5 years      534400                 6.48  2011
##  2 5 to 9 years       473915                 5.75  2011
##  3 10 to 14 years     467219                 5.67  2011
##  4 15 to 19 years     508178                 6.16  2011
##  5 20 to 24 years     643544                 7.81  2011
##  6 25 to 34 years    1423451                17.3   2011
##  7 35 to 44 years    1151332                14.0   2011
##  8 45 to 54 years    1109379                13.5   2011
##  9 55 to 59 years     495114                 6.01  2011
## 10 60 to 64 years     426382                 5.17  2011
## # … with 55 more rows
```


```r
demo_race_list <- list(demo_2011_race, demo_2012_race, demo_2013_race, demo_2014_race, demo_2015_race)
demo_race_all <- bind_rows(demo_race_list)
demo_race_all
```

```
## # A tibble: 35 × 4
##    race                                        population percent_pop_for…  year
##    <chr>                                            <dbl>            <dbl> <dbl>
##  1 "White"                                        3618421          43.9     2011
##  2 "Black or African American"                    2054101          24.9     2011
##  3 "American Indian and Alaska Native"              27390           0.332   2011
##  4 "Asian"                                        1051946          12.8     2011
##  5 "Native Hawaiian and Other Pacific Islande…       4321           0.0524  2011
##  6 "Some other race"                              1236450          15.0     2011
##  7 "Two or more races"                             252281           3.06    2011
##  8 "    White"                                    3647131          43.7     2012
##  9 "    Black or African American"                2058522          24.7     2012
## 10 "    American Indian and Alaska Native"          30230           0.363   2012
## # … with 25 more rows
```


```r
write_csv(demo_age_all, "demo_age_all.csv")
write_csv(demo_race_all, "demo_race_all.csv")
```

