---
title: "Epidemology: HIV/AIDS"
author: "Benjamin Prieto"
date: "2/15/2022"
output: 
  html_document: 
    keep_md: yes
---

My Focus: HIV and AIDS, disproportional affected groups/demographics over time

# Setup

## Load Packages

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

```
## 
## Attaching package: 'janitor'
```

```
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

```
## 
## Attaching package: 'shinydashboard'
```

```
## The following object is masked from 'package:graphics':
## 
##     box
```

## Load Data  

### NYC HIV/AIDS Data

[Data Source: NYC HIV/AIDS Data (2011-2015)](https://data.world/city-of-ny/fju2-rdad)

```r
data_raw <- read_csv("data/dohmh-hiv-aids-annual-report-1.csv") 
```

```
## Rows: 6005 Columns: 18
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (5): Borough, UHF, Gender, Age, Race
## dbl (13): Year, HIV diagnoses, HIV diagnosis rate, Concurrent diagnoses, % l...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
data_raw
```

```
## # A tibble: 6,005 × 18
##     Year Borough UHF   Gender      Age    Race  `HIV diagnoses` `HIV diagnosis…`
##    <dbl> <chr>   <chr> <chr>       <chr>  <chr>           <dbl>            <dbl>
##  1  2011 All     All   All         All    All              3379             48.3
##  2  2011 All     All   Male        All    All              2595             79.1
##  3  2011 All     All   Female      All    All               733             21.1
##  4  2011 All     All   Transgender All    All                51          99999  
##  5  2011 All     All   Female      13 - … All                47             13.6
##  6  2011 All     All   Female      20 - … All               178             24.7
##  7  2011 All     All   Female      30 - … All               176             26.9
##  8  2011 All     All   Female      40 - … All               195             33  
##  9  2011 All     All   Female      50 - … All               130             23.5
## 10  2011 All     All   Female      60+    All                57              6.7
## # … with 5,995 more rows, and 10 more variables: `Concurrent diagnoses` <dbl>,
## #   `% linked to care within 3 months` <dbl>, `AIDS diagnoses` <dbl>,
## #   `AIDS diagnosis rate` <dbl>, `PLWDHI prevalence` <dbl>,
## #   `% viral suppression` <dbl>, Deaths <dbl>, `Death rate` <dbl>,
## #   `HIV-related death rate` <dbl>, `Non-HIV-related death rate` <dbl>
```

### Raw Data Overview


```r
glimpse(data_raw)
```

```
## Rows: 6,005
## Columns: 18
## $ Year                               <dbl> 2011, 2011, 2011, 2011, 2011, 2011,…
## $ Borough                            <chr> "All", "All", "All", "All", "All", …
## $ UHF                                <chr> "All", "All", "All", "All", "All", …
## $ Gender                             <chr> "All", "Male", "Female", "Transgend…
## $ Age                                <chr> "All", "All", "All", "All", "13 - 1…
## $ Race                               <chr> "All", "All", "All", "All", "All", …
## $ `HIV diagnoses`                    <dbl> 3379, 2595, 733, 51, 47, 178, 176, …
## $ `HIV diagnosis rate`               <dbl> 48.3, 79.1, 21.1, 99999.0, 13.6, 24…
## $ `Concurrent diagnoses`             <dbl> 640, 480, 153, 7, 4, 20, 31, 50, 32…
## $ `% linked to care within 3 months` <dbl> 66, 66, 66, 63, 64, 67, 66, 62, 72,…
## $ `AIDS diagnoses`                   <dbl> 2366, 1712, 622, 32, 22, 96, 133, 2…
## $ `AIDS diagnosis rate`              <dbl> 33.8, 52.2, 17.6, 99999.0, 6.4, 13.…
## $ `PLWDHI prevalence`                <dbl> 1.1, 1.7, 0.6, 99999.0, 0.1, 0.3, 0…
## $ `% viral suppression`              <dbl> 71, 72, 68, 55, 57, 48, 61, 66, 73,…
## $ Deaths                             <dbl> 2040, 1423, 605, 12, 1, 19, 53, 184…
## $ `Death rate`                       <dbl> 13.6, 13.4, 14.0, 11.1, 1.4, 7.2, 9…
## $ `HIV-related death rate`           <dbl> 5.8, 5.7, 6.0, 5.7, 1.4, 3.2, 5.7, …
## $ `Non-HIV-related death rate`       <dbl> 7.8, 7.7, 8.0, 5.4, 0.0, 4.0, 3.7, …
```


```r
miss_var_summary(data_raw)
```

```
## # A tibble: 18 × 3
##    variable                         n_miss pct_miss
##    <chr>                             <int>    <dbl>
##  1 Year                                  0        0
##  2 Borough                               0        0
##  3 UHF                                   0        0
##  4 Gender                                0        0
##  5 Age                                   0        0
##  6 Race                                  0        0
##  7 HIV diagnoses                         0        0
##  8 HIV diagnosis rate                    0        0
##  9 Concurrent diagnoses                  0        0
## 10 % linked to care within 3 months      0        0
## 11 AIDS diagnoses                        0        0
## 12 AIDS diagnosis rate                   0        0
## 13 PLWDHI prevalence                     0        0
## 14 % viral suppression                   0        0
## 15 Deaths                                0        0
## 16 Death rate                            0        0
## 17 HIV-related death rate                0        0
## 18 Non-HIV-related death rate            0        0
```


```r
summary(data_raw)
```

```
##       Year        Borough              UHF               Gender         
##  Min.   :2011   Length:6005        Length:6005        Length:6005       
##  1st Qu.:2012   Class :character   Class :character   Class :character  
##  Median :2013   Mode  :character   Mode  :character   Mode  :character  
##  Mean   :2013                                                           
##  3rd Qu.:2014                                                           
##  Max.   :2015                                                           
##      Age                Race           HIV diagnoses    HIV diagnosis rate
##  Length:6005        Length:6005        Min.   :   0.0   Min.   :    0.0   
##  Class :character   Class :character   1st Qu.:   0.0   1st Qu.:    0.0   
##  Mode  :character   Mode  :character   Median :   3.0   Median :   18.5   
##                                        Mean   :  26.5   Mean   :  119.5   
##                                        3rd Qu.:  13.0   3rd Qu.:   49.4   
##                                        Max.   :3379.0   Max.   :99999.0   
##  Concurrent diagnoses % linked to care within 3 months AIDS diagnoses   
##  Min.   :  0.000      Min.   :    0                    Min.   :    0.0  
##  1st Qu.:  0.000      1st Qu.:   67                    1st Qu.:    0.0  
##  Median :  1.000      Median :   83                    Median :    2.0  
##  Mean   :  5.095      Mean   :25399                    Mean   :   33.3  
##  3rd Qu.:  3.000      3rd Qu.:99999                    3rd Qu.:    8.0  
##  Max.   :640.000      Max.   :99999                    Max.   :99999.0  
##  AIDS diagnosis rate PLWDHI prevalence % viral suppression     Deaths        
##  Min.   :    0.0     Min.   :    0.0   Min.   :    0       Min.   :    0.00  
##  1st Qu.:    0.0     1st Qu.:    0.2   1st Qu.:   71       1st Qu.:    0.00  
##  Median :   10.4     Median :    0.6   Median :   79       Median :    1.00  
##  Mean   :  122.8     Mean   :  317.5   Mean   : 2656       Mean   :   49.45  
##  3rd Qu.:   30.6     3rd Qu.:    1.5   3rd Qu.:   87       3rd Qu.:    8.00  
##  Max.   :99999.0     Max.   :99999.0   Max.   :99999       Max.   :99999.00  
##    Death rate     HIV-related death rate Non-HIV-related death rate
##  Min.   :  0.00   Min.   :    0.0        Min.   :    0.0           
##  1st Qu.:  0.00   1st Qu.:    0.0        1st Qu.:    0.0           
##  Median :  6.00   Median :    3.0        Median :    5.5           
##  Mean   : 10.34   Mean   :20003.2        Mean   :20005.1           
##  3rd Qu.: 14.10   3rd Qu.:   14.4        3rd Qu.:   22.1           
##  Max.   :263.20   Max.   :99999.0        Max.   :99999.0
```


### Data Cleanup and Clean Data Overview


```r
data_clean <- data_raw %>% 
  clean_names() %>% 
  na_if("99999") %>% 
  filter(gender != "All",
         borough != "All",
         uhf != "All") %>% 
  select(-uhf, -percent_linked_to_care_within_3_months) #remove variables not of interest
data_clean
```

```
## # A tibble: 5,040 × 16
##     year borough gender age     race              hiv_diagnoses hiv_diagnosis_r…
##    <dbl> <chr>   <chr>  <chr>   <chr>                     <dbl>            <dbl>
##  1  2011 Bronx   Female 13 - 19 All                           1              8.1
##  2  2011 Bronx   Female 20 - 29 All                           5             27.3
##  3  2011 Bronx   Female 30 - 39 All                          12             78  
##  4  2011 Bronx   Female 40 - 49 All                           7             44.5
##  5  2011 Bronx   Female 50 - 59 All                           5             38.5
##  6  2011 Bronx   Female 60+     All                           1              7  
##  7  2011 Bronx   Female All     All                          31             34.8
##  8  2011 Bronx   Female All     Asian/Pacific Is…             0              0  
##  9  2011 Bronx   Female All     Black                        16             60.7
## 10  2011 Bronx   Female All     Latino/Hispanic              13             22  
## # … with 5,030 more rows, and 9 more variables: concurrent_diagnoses <dbl>,
## #   aids_diagnoses <dbl>, aids_diagnosis_rate <dbl>, plwdhi_prevalence <dbl>,
## #   percent_viral_suppression <dbl>, deaths <dbl>, death_rate <dbl>,
## #   hiv_related_death_rate <dbl>, non_hiv_related_death_rate <dbl>
```

*Note: `age` is compiled based on `race` and vice versa, so no analyses can be done looking at the intersection of age and race. Careful with these two variables during analyses.

### Clean Data Overview


```r
glimpse(data_clean)
```

```
## Rows: 5,040
## Columns: 16
## $ year                       <dbl> 2011, 2011, 2011, 2011, 2011, 2011, 2011, 2…
## $ borough                    <chr> "Bronx", "Bronx", "Bronx", "Bronx", "Bronx"…
## $ gender                     <chr> "Female", "Female", "Female", "Female", "Fe…
## $ age                        <chr> "13 - 19", "20 - 29", "30 - 39", "40 - 49",…
## $ race                       <chr> "All", "All", "All", "All", "All", "All", "…
## $ hiv_diagnoses              <dbl> 1, 5, 12, 7, 5, 1, 31, 0, 16, 13, 0, 2, 4, …
## $ hiv_diagnosis_rate         <dbl> 8.1, 27.3, 78.0, 44.5, 38.5, 7.0, 34.8, 0.0…
## $ concurrent_diagnoses       <dbl> 0, 1, 1, 5, 1, 0, 8, 0, 6, 2, 0, 0, 0, 3, 4…
## $ aids_diagnoses             <dbl> 1, 1, 11, 16, 8, 3, 40, 0, 26, 14, 0, 0, 2,…
## $ aids_diagnosis_rate        <dbl> 8.1, 5.5, 71.5, 101.8, 61.6, 20.9, 44.9, 0.…
## $ plwdhi_prevalence          <dbl> 0.2, 0.6, 1.8, 3.4, 3.5, 1.2, 1.8, 0.5, 2.9…
## $ percent_viral_suppression  <dbl> 33, 49, 60, 60, 73, 80, 65, 71, 66, 65, 100…
## $ deaths                     <dbl> 0, 4, 6, 11, 12, 8, 41, 0, 23, 17, 0, 1, 0,…
## $ death_rate                 <dbl> 0.0, 26.1, 16.5, 14.5, 21.0, 29.9, 16.8, 0.…
## $ hiv_related_death_rate     <dbl> 0.0, 13.1, 11.0, 4.4, 12.3, 25.6, 10.5, 0.0…
## $ non_hiv_related_death_rate <dbl> 0.0, 13.1, 5.5, 10.2, 8.8, 4.3, 6.4, 0.0, 9…
```


```r
miss_var_summary(data_clean)
```

```
## # A tibble: 16 × 3
##    variable                   n_miss pct_miss
##    <chr>                       <int>    <dbl>
##  1 hiv_related_death_rate       1008  20     
##  2 non_hiv_related_death_rate   1008  20     
##  3 percent_viral_suppression     155   3.08  
##  4 plwdhi_prevalence              14   0.278 
##  5 deaths                          2   0.0397
##  6 aids_diagnoses                  1   0.0198
##  7 aids_diagnosis_rate             1   0.0198
##  8 year                            0   0     
##  9 borough                         0   0     
## 10 gender                          0   0     
## 11 age                             0   0     
## 12 race                            0   0     
## 13 hiv_diagnoses                   0   0     
## 14 hiv_diagnosis_rate              0   0     
## 15 concurrent_diagnoses            0   0     
## 16 death_rate                      0   0
```


```r
summary(data_clean)
```

```
##       year        borough             gender              age           
##  Min.   :2011   Length:5040        Length:5040        Length:5040       
##  1st Qu.:2012   Class :character   Class :character   Class :character  
##  Median :2013   Mode  :character   Mode  :character   Mode  :character  
##  Mean   :2013                                                           
##  3rd Qu.:2014                                                           
##  Max.   :2015                                                           
##                                                                         
##      race           hiv_diagnoses     hiv_diagnosis_rate concurrent_diagnoses
##  Length:5040        Min.   :  0.000   Min.   :  0.00     Min.   : 0.000      
##  Class :character   1st Qu.:  0.000   1st Qu.:  0.00     1st Qu.: 0.000      
##  Mode  :character   Median :  2.000   Median : 17.10     Median : 0.000      
##                     Mean   :  7.479   Mean   : 36.65     Mean   : 1.464      
##                     3rd Qu.:  7.000   3rd Qu.: 49.80     3rd Qu.: 2.000      
##                     Max.   :161.000   Max.   :635.30     Max.   :35.000      
##                                                                              
##  aids_diagnoses    aids_diagnosis_rate plwdhi_prevalence
##  Min.   :  0.000   Min.   :  0.00      Min.   : 0.000   
##  1st Qu.:  0.000   1st Qu.:  0.00      1st Qu.: 0.200   
##  Median :  1.000   Median :  9.30      Median : 0.500   
##  Mean   :  4.678   Mean   : 23.08      Mean   : 1.131   
##  3rd Qu.:  5.000   3rd Qu.: 30.40      3rd Qu.: 1.500   
##  Max.   :127.000   Max.   :494.50      Max.   :15.300   
##  NA's   :1         NA's   :1           NA's   :14       
##  percent_viral_suppression     deaths          death_rate   
##  Min.   :  0.00            Min.   :  0.000   Min.   :  0.0  
##  1st Qu.: 70.00            1st Qu.:  0.000   1st Qu.:  0.0  
##  Median : 79.00            Median :  1.000   Median :  4.5  
##  Mean   : 76.83            Mean   :  4.476   Mean   : 10.2  
##  3rd Qu.: 87.00            3rd Qu.:  4.000   3rd Qu.: 14.1  
##  Max.   :100.00            Max.   :115.000   Max.   :263.2  
##  NA's   :155               NA's   :2                        
##  hiv_related_death_rate non_hiv_related_death_rate
##  Min.   :  0.000        Min.   :  0.000           
##  1st Qu.:  0.000        1st Qu.:  0.000           
##  Median :  0.000        Median :  0.000           
##  Mean   :  4.164        Mean   :  6.494           
##  3rd Qu.:  4.900        3rd Qu.:  8.500           
##  Max.   :172.200        Max.   :210.500           
##  NA's   :1008           NA's   :1008
```


```r
names(data_clean)
```

```
##  [1] "year"                       "borough"                   
##  [3] "gender"                     "age"                       
##  [5] "race"                       "hiv_diagnoses"             
##  [7] "hiv_diagnosis_rate"         "concurrent_diagnoses"      
##  [9] "aids_diagnoses"             "aids_diagnosis_rate"       
## [11] "plwdhi_prevalence"          "percent_viral_suppression" 
## [13] "deaths"                     "death_rate"                
## [15] "hiv_related_death_rate"     "non_hiv_related_death_rate"
```


### HIV Data Breakdown

[Source: Dataset Variables Overview](https://data.cityofnewyork.us/Health/DOHMH-HIV-AIDS-Annual-Report/fju2-rdad)  

**Summary**  

HIV/AIDS data from the HIV Surveillance Annual Report * Note: Data reported to the HIV Epidemiology and Field Services Program by June 30, 2016. All data shown are for people ages 13 and older. Borough-wide and citywide totals may include cases assigned to a borough with an unknown UHF or assigned to NYC with an unknown borough, respectively. Therefore, UHF totals may not sum to borough totals and borough totals may not sum to citywide totals."

*Year* = Self-explanatory  

*Borough* = Borough of residence at HIV diagnosis for HIV diagnoses, HIV diagnosis rate, Concurrent diagnoses, and % linked to care within 3 months; Borough of residence at AIDS diagnosis for AIDS diagnoses and AIDS diagnosis rates; Borough of residence at most recent address for PLWDHI prevalence and % viral suppression; Borough of residence at death for Deaths and all death rates.  

*UHF (removed)* = United Hospital Fund neighborhood of residence at HIV diagnosis for HIV diagnoses, HIV diagnosis rates, Concurrent diagnoses, and % Linked to care within 3 months; UHF of residence at AIDS diagnosis for AIDS diagnoses and AIDS diagnosis rates; UHF of residence at most recent address for PLWDHI prevalence and % viral suppression; UHF of residence at death for Deaths and all death rates.  

*Gender* = Gender: when Borough, UHF, Age, and Race categories are all equal to 'All', Gender is displayed as Male, Female, and Transgender (mutually exclusive categories). In all other rows, Male includes transgender men and Female includes transgender women.  

*Age* = Age at HIV diagnosis for HIV diagnoses, HIV diagnosis rates, Concurrent diagnoses, and % Linked to care within 3 months; Age at AIDS diagnosis for AIDS diagnoses and AIDS diagnosis rates; Age as of the end of the given year for PLWDHI prevalence and % viral suppression; Age at death for Deaths and all death rates.
*Race* = Race/ethnicity; Other/Unknown category includes people of Native American, multiracial, and unknown races.  

*HIV Diagnoses* = Number of HIV diagnoses 13 years of age or older, including those concurrent with AIDS, excluding people known to have been diagnosed outside of NYC  

*HIV Diagnoses Rate* = HIV diagnoses per 100,000 NYC population using annual intercensal population estimates *‘99999’ value indicates suppressed cell representing 1-5 person(s) with an underlying denominator of <=500 people or non-zero cell with a denominator <=100.  

*Concurrent Diagnoses* = Number of HIV diagnoses 13 years of age or older with a concurrent AIDS diagnosis (within 31 days)  

*% Linked to Care Within 3 Months (removed)* = Proportion of new HIV diagnoses with an HIV viral load or CD4 test drawn within 3 months (91 days) of HIV diagnosis, following a 7-day lag *‘99999’ value indicates proportion is not calculated because the underlying denominator is equal to zero or is unknown.  

*AIDS Diagnoses* = Number of AIDS diagnoses 13 years of age or older. *‘99999’ value indicates suppressed cell representing 1-5 person(s) with an underlying denominator of <=500 people or non-zero cell with a denominator <=100.  

*AIDS Diagnoses Rate* = AIDS diagnoses per 100,000 NYC population using annual intercensal population estimates. *‘99999’ value indicates rate is not calculated because the underlying denominator is equal to zero or is unknown.  

*PLWDHI Preference* = Estimated number of people 13 years of age or older living with diagnosed HIV infection (PLWDHI) per 100 NYC population using annual intercensal population estimates. *‘99999’ value indicates rate is not calculated because the underlying denominator is equal to zero or is unknown.  

*% Viral Suppression* = Proportion of people living with diagnosed HIV infection 13 years of age or older with at least one viral load test during the calendar year whose last HIV viral load value was ≤200 copies/mL. *‘99999’ value indicates proportion is not calculated because the underlying denominator is equal to zero or is unknown.  

*Deaths* = Number of deaths from any cause among people with HIV/AIDS 13 years of age or older. *‘99999’ value indicates suppressed cell representing 1-5 person(s) with an underlying denominator of <=500 people or non-zero cell with a denominator <=100.  

*Death Rate* = Deaths per 1,000 mid-year people living with HIV/AIDS, excluding deaths in which the person was diagnosed with HIV at the time of death or up to 15 days prior to death. Deaths are age-adjusted to the NYC Census 2010 population and include deaths from any cause (including unknown causes).  

*HIV-related Death Rate* = Death rate for those assigned an HIV-related cause of death. *‘99999’ value indicates rate is not calculated because the underlying denominator is equal to zero or is unknown.  

*Non HIV-related Death Rate* = Death rate for those assigned a non-HIV-related cause of death. *‘99999’ value indicates rate is not calculated because the underlying denominator is equal to zero or is unknown.  

### NYC Demographics Data (2011-2015)

[Data Source: NYC Demographics (2011-2015)](https://www1.nyc.gov/site/planning/planning-level/nyc-population/american-community-survey.page.page)  

Data frames of New York City census data regarding age and race from 2011 to 2015 was compiled in a separate rmd in the nyc_demographics folder (`nyc_demographics.Rmd`) to keep this rmd clean and focused on core focus analyses. These data frames are for comparison between proportions among demographics in the HIV data and the NYC population demographics proportions.

```r
demo_age_all <- read.csv("data/demo_age_all.csv")
demo_age_all
```

```
##            age_group population percent_pop_for_year year
## 1      under 5 years     534400             6.481575 2011
## 2       5 to 9 years     473915             5.747971 2011
## 3     10 to 14 years     467219             5.666757 2011
## 4     15 to 19 years     508178             6.163536 2011
## 5     20 to 24 years     643544             7.805349 2011
## 6     25 to 34 years    1423451            17.264603 2011
## 7     35 to 44 years    1151332            13.964155 2011
## 8     45 to 54 years    1109379            13.455320 2011
## 9     55 to 59 years     495114             6.005087 2011
## 10    60 to 64 years     426382             5.171457 2011
## 11    65 to 74 years     544766             6.607301 2011
## 12    75 to 84 years     323957             3.929176 2011
## 13 85 years and over     143273             1.737715 2011
## 14     under 5 years     544369             6.529792 2012
## 15      5 to 9 years     476525             5.715993 2012
## 16    10 to 14 years     471011             5.649851 2012
## 17    15 to 19 years     498696             5.981937 2012
## 18    20 to 24 years     643802             7.722507 2012
## 19    25 to 34 years    1450056            17.393651 2012
## 20    35 to 44 years    1161456            13.931849 2012
## 21    45 to 54 years    1108238            13.293490 2012
## 22    55 to 59 years     493406             5.918483 2012
## 23    60 to 64 years     443050             5.314455 2012
## 24    65 to 74 years     572984             6.873034 2012
## 25    75 to 84 years     320241             3.841341 2012
## 26 85 years and over     152863             1.833616 2012
## 27     under 5 years     555756             6.611549 2013
## 28      5 to 9 years     479102             5.699635 2013
## 29    10 to 14 years     466303             5.547371 2013
## 30    15 to 19 years     481367             5.726580 2013
## 31    20 to 24 years     634623             7.549790 2013
## 32    25 to 34 years    1478414            17.587945 2013
## 33    35 to 44 years    1170936            13.930035 2013
## 34    45 to 54 years    1112156            13.230759 2013
## 35    55 to 59 years     505267             6.010906 2013
## 36    60 to 64 years     447366             5.322087 2013
## 37    65 to 74 years     593249             7.057584 2013
## 38    75 to 84 years     325287             3.869775 2013
## 39 85 years and over     156011             1.855984 2013
## 40     under 5 years     567142             6.679269 2014
## 41      5 to 9 years     492017             5.794517 2014
## 42    10 to 14 years     456957             5.381613 2014
## 43    15 to 19 years     473714             5.578961 2014
## 44    20 to 24 years     626933             7.383431 2014
## 45    25 to 34 years    1504601            17.719786 2014
## 46    35 to 44 years    1181189            13.910941 2014
## 47    45 to 54 years    1117471            13.160530 2014
## 48    55 to 59 years     521993             6.147546 2014
## 49    60 to 64 years     450669             5.307559 2014
## 50    65 to 74 years     612532             7.213830 2014
## 51    75 to 84 years     328189             3.865104 2014
## 52 85 years and over     157672             1.856914 2014
## 53     under 5 years     569712             6.662983 2015
## 54      5 to 9 years     482350             5.641253 2015
## 55    10 to 14 years     471932             5.519411 2015
## 56    15 to 19 years     469766             5.494079 2015
## 57    20 to 24 years     608130             7.112295 2015
## 58    25 to 34 years    1526724            17.855575 2015
## 59    35 to 44 years    1186890            13.881097 2015
## 60    45 to 54 years    1118835            13.085170 2015
## 61    55 to 59 years     523518             6.122728 2015
## 62    60 to 64 years     465037             5.438772 2015
## 63    65 to 74 years     635453             7.431847 2015
## 64    75 to 84 years     337914             3.952023 2015
## 65 85 years and over     154144             1.802768 2015
```


```r
demo_race_all <- read_csv("data/demo_race_all.csv")
```

```
## Rows: 35 Columns: 4
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): race
## dbl (3): population, percent_pop_for_year, year
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

```r
demo_race_all
```

```
## # A tibble: 35 × 4
##    race                                       population percent_pop_for_…  year
##    <chr>                                           <dbl>             <dbl> <dbl>
##  1 White                                         3618421           43.9     2011
##  2 Black or African American                     2054101           24.9     2011
##  3 American Indian and Alaska Native               27390            0.332   2011
##  4 Asian                                         1051946           12.8     2011
##  5 Native Hawaiian and Other Pacific Islander       4321            0.0524  2011
##  6 Some other race                               1236450           15.0     2011
##  7 Two or more races                              252281            3.06    2011
##  8 White                                         3647131           43.7     2012
##  9 Black or African American                     2058522           24.7     2012
## 10 American Indian and Alaska Native               30230            0.363   2012
## # … with 25 more rows
```


# Analyses

## Age and HIV

HIV Diagnoses by Age: Are HIV diagnoses disproportional across age groups?

```r
data_clean %>% 
  filter(age != "All") %>% 
  group_by(age) %>% 
  summarize(sum_hiv_diagnoses = sum(hiv_diagnoses))
```

```
## # A tibble: 6 × 2
##   age     sum_hiv_diagnoses
##   <chr>               <dbl>
## 1 13 - 19               537
## 2 20 - 29              4315
## 3 30 - 39              3117
## 4 40 - 49              2490
## 5 50 - 59              1451
## 6 60+                   654
```


```r
data_clean %>% 
  filter(age != "All") %>% 
  group_by(age) %>% 
  summarize(sum_hiv_diagnoses = sum(hiv_diagnoses)) %>% 
  ggplot(aes(x = age, y = sum_hiv_diagnoses, fill = age))+
  geom_col()+
  labs(x = "Age Group",
       y = "HIV Diagnoses",
       title = "HIV Diagnoses by Age Group (2011-2015)")+
  theme_stata()+
  theme(legend.position = "none")
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-14-1.png)<!-- -->


HIV Diagnosis vs. Age Over Time: How does HIV diagnoses change over time across all age groups?

Absolute

```r
data_clean %>% 
  filter(age != "All") %>% 
  filter(year == 2011 | year == 2015) %>% 
  select(year, age, hiv_diagnoses) %>% 
  group_by(age, year) %>% 
  summarize(sum_hiv_diagnoses = sum(hiv_diagnoses))
```

```
## `summarise()` has grouped output by 'age'. You can override using the `.groups`
## argument.
```

```
## # A tibble: 12 × 3
## # Groups:   age [6]
##    age      year sum_hiv_diagnoses
##    <chr>   <dbl>             <dbl>
##  1 13 - 19  2011               151
##  2 13 - 19  2015                66
##  3 20 - 29  2011               931
##  4 20 - 29  2015               797
##  5 30 - 39  2011               713
##  6 30 - 39  2015               564
##  7 40 - 49  2011               642
##  8 40 - 49  2015               375
##  9 50 - 59  2011               332
## 10 50 - 59  2015               279
## 11 60+      2011               122
## 12 60+      2015               135
```


```r
data_clean %>% 
  filter(age != "All") %>% 
  filter(year == 2011 | year == 2015) %>% 
  select(year, age, hiv_diagnoses) %>% 
  group_by(age, year) %>% 
  summarize(sum_hiv_diagnoses = sum(hiv_diagnoses)) %>% 
  ggplot(aes(x = age, y = sum_hiv_diagnoses, fill = as.factor(year)))+
  geom_col(position = "dodge")+
  labs(x = "Age Group",
       y = "HIV Diagnoses",
       title = "HIV Diagnoses by Age Group (2011-2015)",
       fill = "Year")+
  theme_stata()+
  scale_fill_brewer(palette = "Paired")
```

```
## `summarise()` has grouped output by 'age'. You can override using the `.groups`
## argument.
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

Over Time

```r
data_clean %>% 
  filter(age != "All") %>% 
  group_by(age, year) %>% 
  summarize(sum_hiv_diagnoses = sum(hiv_diagnoses))
```

```
## `summarise()` has grouped output by 'age'. You can override using the `.groups`
## argument.
```

```
## # A tibble: 30 × 3
## # Groups:   age [6]
##    age      year sum_hiv_diagnoses
##    <chr>   <dbl>             <dbl>
##  1 13 - 19  2011               151
##  2 13 - 19  2012               128
##  3 13 - 19  2013               104
##  4 13 - 19  2014                88
##  5 13 - 19  2015                66
##  6 20 - 29  2011               931
##  7 20 - 29  2012               906
##  8 20 - 29  2013               889
##  9 20 - 29  2014               792
## 10 20 - 29  2015               797
## # … with 20 more rows
```


```r
data_clean %>% 
  filter(age != "All") %>% 
  group_by(age, year) %>% 
  summarize(sum_hiv_diagnoses = sum(hiv_diagnoses)) %>% 
  ggplot(aes(x = year, y = sum_hiv_diagnoses, color = age))+
  geom_line()+
  labs(x = "Year",
       y = "HIV Diagnoses",
       title = "HIV Diagnoses Over Time by Age Group (2011-2015)",
       color = NULL)+
  theme_stata()
```

```
## `summarise()` has grouped output by 'age'. You can override using the `.groups`
## argument.
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-18-1.png)<!-- -->


```r
data_clean %>% 
  filter(age != "All") %>% 
  group_by(age, year) %>% 
  summarize(sum_hiv_diagnoses = sum(hiv_diagnoses)) %>% 
  ggplot(aes(x = year, y = sum_hiv_diagnoses, fill = age))+
  geom_col(alpha = 0.9)+
  labs(x = "Year",
       y = "HIV Diagnoses",
       title = "HIV Diagnoses Over Time by Age Group (2011-2015)",
       fill = NULL)+
  theme_stata()
```

```
## `summarise()` has grouped output by 'age'. You can override using the `.groups`
## argument.
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-19-1.png)<!-- -->

AIDS Diagnoses by Age: Are AIDS diagnoses disproportional across age groups?

```r
data_clean %>% 
  filter(age != "All") %>% 
  group_by(age) %>% 
  summarize(sum_aids_diagnoses = sum(aids_diagnoses))
```

```
## # A tibble: 6 × 2
##   age     sum_aids_diagnoses
##   <chr>                <dbl>
## 1 13 - 19                132
## 2 20 - 29               1410
## 3 30 - 39               1805
## 4 40 - 49               2204
## 5 50 - 59               1609
## 6 60+                    697
```


```r
data_clean %>% 
  filter(age != "All") %>% 
  group_by(age) %>% 
  summarize(sum_aids_diagnoses = sum(aids_diagnoses)) %>% 
  ggplot(aes(x = age, y = sum_aids_diagnoses, fill = age))+
  geom_col()+
  labs(x = "Age Group",
       y = "AIDS Diagnoses",
       title = "AIDS Diagnoses by Age Group(2011-2015)",
       fill = NULL)+
  theme_stata()+
  theme(legend.position = "none")
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-21-1.png)<!-- -->

AIDS Diagnoses by Age Over Time: Are AIDS diagnoses disproportional across age groups over time?

Absolute

```r
data_clean %>% 
  filter(age != "All") %>% 
  filter(year == 2011 | year == 2015) %>% 
  group_by(age, year) %>% 
  summarize(sum_aids_diagnoses = sum(aids_diagnoses))
```

```
## `summarise()` has grouped output by 'age'. You can override using the `.groups`
## argument.
```

```
## # A tibble: 12 × 3
## # Groups:   age [6]
##    age      year sum_aids_diagnoses
##    <chr>   <dbl>              <dbl>
##  1 13 - 19  2011                 47
##  2 13 - 19  2015                 20
##  3 20 - 29  2011                370
##  4 20 - 29  2015                192
##  5 30 - 39  2011                466
##  6 30 - 39  2015                280
##  7 40 - 49  2011                637
##  8 40 - 49  2015                275
##  9 50 - 59  2011                384
## 10 50 - 59  2015                237
## 11 60+      2011                144
## 12 60+      2015                122
```


```r
data_clean %>% 
  filter(age != "All") %>% 
  filter(year == 2011 | year == 2015) %>% 
  group_by(age, year) %>% 
  summarize(sum_aids_diagnoses = sum(aids_diagnoses)) %>% 
  ggplot(aes(x = age, y = sum_aids_diagnoses, fill = as.factor(year)))+
  geom_col(position = "dodge")+
  labs(x = "Age Group",
       y = "AIDS Diagnoses",
       title = "AIDS Diagnoses by Age Agroup (2011-2015)",
       fill = "Year")+
  theme_stata()+
  scale_fill_brewer(palette = "Paired")
```

```
## `summarise()` has grouped output by 'age'. You can override using the `.groups`
## argument.
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-23-1.png)<!-- -->


Over Time

```r
data_clean %>% 
  filter(age != "All") %>% 
  group_by(age, year) %>% 
  summarize(sum_aids_diagnoses = sum(aids_diagnoses))
```

```
## `summarise()` has grouped output by 'age'. You can override using the `.groups`
## argument.
```

```
## # A tibble: 30 × 3
## # Groups:   age [6]
##    age      year sum_aids_diagnoses
##    <chr>   <dbl>              <dbl>
##  1 13 - 19  2011                 47
##  2 13 - 19  2012                 27
##  3 13 - 19  2013                 24
##  4 13 - 19  2014                 14
##  5 13 - 19  2015                 20
##  6 20 - 29  2011                370
##  7 20 - 29  2012                332
##  8 20 - 29  2013                319
##  9 20 - 29  2014                197
## 10 20 - 29  2015                192
## # … with 20 more rows
```


```r
data_clean %>% 
  filter(age != "All") %>% 
  group_by(age, year) %>% 
  summarize(sum_aids_diagnoses = sum(aids_diagnoses)) %>% 
  ggplot(aes(x = year, y = sum_aids_diagnoses, color = age))+
  geom_line()+
  labs(x = "Year",
       y = "AIDS Diagnoses",
       title = "AIDS Diagnoses by Age Group (2011-2015)",
       color = NULL)+
  theme_stata()
```

```
## `summarise()` has grouped output by 'age'. You can override using the `.groups`
## argument.
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-25-1.png)<!-- -->


```r
data_clean %>% 
  filter(age != "All") %>% 
  group_by(age, year) %>% 
  summarize(sum_aids_diagnoses = sum(aids_diagnoses)) %>% 
  ggplot(aes(x = year, y = sum_aids_diagnoses, fill = age))+
  geom_col()+
  labs(x = "Year",
       y = "AIDS Diagnoses",
       title = "AIDS Diagnoses by Age Group (2011-2015)",
       fill = NULL)+
  theme_stata()
```

```
## `summarise()` has grouped output by 'age'. You can override using the `.groups`
## argument.
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-26-1.png)<!-- -->


HIV-related Death Rate by Age Group: Is HIV-related death more or less common across different age groups?  

Expectation: Death rate is more common in older age groups due to age-associated health decline.


```r
data_clean %>% 
  filter(age != "All") %>% 
  group_by(age) %>% 
  summarize(avg_hiv_related_death_rate = mean(hiv_related_death_rate, na.rm = T))
```

```
## # A tibble: 6 × 2
##   age     avg_hiv_related_death_rate
##   <chr>                        <dbl>
## 1 13 - 19                      0.424
## 2 20 - 29                      2.85 
## 3 30 - 39                      3.24 
## 4 40 - 49                      4.53 
## 5 50 - 59                      6.47 
## 6 60+                         10.2
```


```r
data_clean %>% 
  filter(age != "All") %>% 
  group_by(age) %>% 
  summarize(avg_hiv_related_death_rate = mean(hiv_related_death_rate, na.rm = T)) %>% 
  ggplot(aes(x = age, y = avg_hiv_related_death_rate, fill = age))+
  geom_col()+
  labs(x = "Age Group",
       y = "Average HIV-Related Death Rate (%)",
       title = "HIV-Related Death Rate by Age Group (2011-2015)",
       fill = "Age")+
  theme_stata()+
  theme(legend.position = "none")
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-28-1.png)<!-- -->

HIV-related Death Rate by Age Group over time: Is HIV-related death more or less common across different age groups over time?

Absolute

```r
data_clean %>% 
  filter(age != "All") %>% 
  filter(year == 2011 | year == 2014) %>% 
  select(year, age, hiv_related_death_rate) %>% 
  group_by(age, year) %>% 
  summarize(avg_hiv_related_death_rate = mean(hiv_related_death_rate))
```

```
## `summarise()` has grouped output by 'age'. You can override using the `.groups`
## argument.
```

```
## # A tibble: 12 × 3
## # Groups:   age [6]
##    age      year avg_hiv_related_death_rate
##    <chr>   <dbl>                      <dbl>
##  1 13 - 19  2011                      0.893
##  2 13 - 19  2014                      0    
##  3 20 - 29  2011                      2.38 
##  4 20 - 29  2014                      2.30 
##  5 30 - 39  2011                      4.01 
##  6 30 - 39  2014                      3.41 
##  7 40 - 49  2011                      6.00 
##  8 40 - 49  2014                      2.51 
##  9 50 - 59  2011                      8.62 
## 10 50 - 59  2014                      5.83 
## 11 60+      2011                     13.6  
## 12 60+      2014                      8.41
```


```r
data_clean %>% 
  filter(age != "All") %>% 
  filter(year == 2011 | year == 2014) %>% 
  select(year, age, hiv_related_death_rate) %>% 
  group_by(age, year) %>% 
  summarize(avg_hiv_related_death_rate = mean(hiv_related_death_rate)) %>% 
  ggplot(aes(x = age, y = avg_hiv_related_death_rate, fill = as.factor(year)))+
  geom_col(position = "dodge")+
  labs(x = "Age Group",
       y = "Average HIV-Related Death Rate (%)",
       title = "HIV-Related Death Rate by Age Group (2011-2014)",
       fill = "Year")+
  theme_stata()+
  scale_fill_brewer(palette = "Paired")
```

```
## `summarise()` has grouped output by 'age'. You can override using the `.groups`
## argument.
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-30-1.png)<!-- -->


Over Time

```r
data_clean %>% 
  filter(age != "All", year != 2015) %>% 
  group_by(age, year) %>% 
  summarize(avg_hiv_related_death_rate = mean(hiv_related_death_rate, na.rm = T))
```

```
## `summarise()` has grouped output by 'age'. You can override using the `.groups`
## argument.
```

```
## # A tibble: 24 × 3
## # Groups:   age [6]
##    age      year avg_hiv_related_death_rate
##    <chr>   <dbl>                      <dbl>
##  1 13 - 19  2011                      0.893
##  2 13 - 19  2012                      0.518
##  3 13 - 19  2013                      0.283
##  4 13 - 19  2014                      0    
##  5 20 - 29  2011                      2.38 
##  6 20 - 29  2012                      4.27 
##  7 20 - 29  2013                      2.46 
##  8 20 - 29  2014                      2.30 
##  9 30 - 39  2011                      4.01 
## 10 30 - 39  2012                      3.82 
## # … with 14 more rows
```


```r
data_clean %>% 
  filter(age != "All", year != 2015) %>% 
  group_by(age, year) %>% 
  summarize(avg_hiv_related_death_rate = mean(hiv_related_death_rate, na.rm = T)) %>% 
  ggplot(aes(x = year, y = avg_hiv_related_death_rate, color = age))+
  geom_line()+
  labs(x = "Year",
       y = "Average HIV-Related Death Rate (%)",
       title = "HIV-Related Death Rate by Age Group (2011-2014)",
       color = NULL)+
  theme_stata()
```

```
## `summarise()` has grouped output by 'age'. You can override using the `.groups`
## argument.
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-32-1.png)<!-- -->

Comparison with NYC Demographics

```r
demo_age_all$age_group <- #order levels by age group instead of alphabetical
  factor(demo_age_all$age_group, 
         levels = c("under 5 years", "5 to 9 years", "10 to 14 years", "15 to 19 years", "20 to 24 years", "25 to 34 years", "35 to 44 years", "45 to 54 years",
                    "55 to 59 years", "60 to 64 years", "65 to 74 years", "75 to 84 years", "85 years and over"))
```


```r
data_clean %>% 
  group_by(age, year) %>% 
  filter(age != "All") %>% 
  summarize(sum_hiv_diagnoses = sum(hiv_diagnoses)) %>% 
  ggplot(aes(x = "", y = sum_hiv_diagnoses, fill = age))+
  geom_col()+
  coord_polar(theta = "y")+
  facet_wrap(~year, nrow = 1)+
  theme_stata()+
  labs(title = "HIV Diagnoses by Age (2011-2015)",
       fill = NULL,
       y = NULL,
       x = "HIV Diagnoses")+
  theme(axis.text.x = element_blank())
```

```
## `summarise()` has grouped output by 'age'. You can override using the `.groups`
## argument.
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-34-1.png)<!-- -->


```r
data_clean %>% 
  group_by(age, year) %>% 
  filter(age != "All") %>% 
  summarize(sum_aids_diagnoses = sum(aids_diagnoses)) %>% 
  ggplot(aes(x = "", y = sum_aids_diagnoses, fill = age))+
  geom_col()+
  coord_polar(theta = "y")+
  facet_wrap(~year, nrow = 1)+
  theme_stata()+
  labs(title = "AIDS Diagnoses by Age (2011-2015)",
       fill = NULL,
       y = NULL,
       x = "AIDS Diagnoses")+
  theme(axis.text.x = element_blank())
```

```
## `summarise()` has grouped output by 'age'. You can override using the `.groups`
## argument.
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-35-1.png)<!-- -->


```r
demo_age_all %>% 
  ggplot(aes(x = "", y = percent_pop_for_year, fill = age_group))+
  geom_col(color = "gray50")+
  coord_polar(theta = "y")+
  facet_wrap(~year, nrow = 1)+
  theme_stata()+
  labs(title = "NYC Population by Age (2011-2015)",
       fill = NULL,
       y = NULL,
       x = "Population")+
  theme(axis.text.x = element_blank(),
        legend.text = element_text(size = 9))+
  scale_fill_brewer(palette = "Set3")
```

```
## Warning in RColorBrewer::brewer.pal(n, pal): n too large, allowed maximum for palette Set3 is 12
## Returning the palette you asked for with that many colors
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-36-1.png)<!-- -->

## Race and HIV

HIV Diagnoses vs. Race: Is HIV diagnoses more or less common across different races?

```r
data_clean %>% 
  group_by(race) %>% 
  filter(race != "All") %>% 
  summarize(sum_hiv_diagnoses = sum(hiv_diagnoses))
```

```
## # A tibble: 5 × 2
##   race                   sum_hiv_diagnoses
##   <chr>                              <dbl>
## 1 Asian/Pacific Islander               524
## 2 Black                               5460
## 3 Latino/Hispanic                     4330
## 4 Other/Unknown                        105
## 5 White                               2145
```


```r
data_clean %>% 
  group_by(race) %>% 
  filter(race != "All") %>% 
  summarize(sum_hiv_diagnoses = sum(hiv_diagnoses)) %>% 
  ggplot(aes(x = race, y = sum_hiv_diagnoses, fill = race))+
  geom_col()+
  labs(x = "Race",
       y = "HIV Diagnoses",
       title = "HIV Diagnoses Proportion by Race (2011-2015)")+
  theme_stata()+
  theme(legend.position = "none")
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-38-1.png)<!-- -->

HIV Diagnoses vs. Race Over Time: Is HIV diagnoses more or less common across different races over time?

Absolute

```r
data_clean %>% 
  filter(race != "All") %>% 
  filter(year == 2011 | year == 2015) %>% 
  group_by(race, year) %>% 
  summarize(sum_hiv_diagnoses = sum(hiv_diagnoses))
```

```
## `summarise()` has grouped output by 'race'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 10 × 3
## # Groups:   race [5]
##    race                    year sum_hiv_diagnoses
##    <chr>                  <dbl>             <dbl>
##  1 Asian/Pacific Islander  2011                96
##  2 Asian/Pacific Islander  2015               106
##  3 Black                   2011              1318
##  4 Black                   2015               933
##  5 Latino/Hispanic         2011               958
##  6 Latino/Hispanic         2015               817
##  7 Other/Unknown           2011                 6
##  8 Other/Unknown           2015                19
##  9 White                   2011               513
## 10 White                   2015               341
```


```r
data_clean %>% 
  filter(race != "All") %>% 
  filter(year == 2011 | year == 2015) %>% 
  group_by(race, year) %>% 
  summarize(sum_hiv_diagnoses = sum(hiv_diagnoses)) %>% 
  ggplot(aes(x = race, y = sum_hiv_diagnoses, fill = as.factor(year)))+
  geom_col(position = "dodge")+
  labs(x = "Race",
       y = "HIV Diagnoses",
       title = "HIV Diagnoses by Race (2011-2015)",
       fill = "Year")+
  theme_stata()+
  scale_fill_brewer(palette = "Paired")
```

```
## `summarise()` has grouped output by 'race'. You can override using the
## `.groups` argument.
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-40-1.png)<!-- -->


Over Time

```r
data_clean %>% 
  group_by(race, year) %>% 
  filter(race != "All") %>% 
  summarize(sum_hiv_diagnoses = sum(hiv_diagnoses))
```

```
## `summarise()` has grouped output by 'race'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 25 × 3
## # Groups:   race [5]
##    race                    year sum_hiv_diagnoses
##    <chr>                  <dbl>             <dbl>
##  1 Asian/Pacific Islander  2011                96
##  2 Asian/Pacific Islander  2012               100
##  3 Asian/Pacific Islander  2013               110
##  4 Asian/Pacific Islander  2014               112
##  5 Asian/Pacific Islander  2015               106
##  6 Black                   2011              1318
##  7 Black                   2012              1168
##  8 Black                   2013              1007
##  9 Black                   2014              1034
## 10 Black                   2015               933
## # … with 15 more rows
```


```r
data_clean %>% 
  group_by(race, year) %>% 
  filter(race != "All") %>% 
  summarize(sum_hiv_diagnoses = sum(hiv_diagnoses)) %>% 
  ggplot(aes(x = year, y = sum_hiv_diagnoses, color = race))+
  geom_line()+
  labs(x = "Year",
       y = "HIV Diagnoses",
       title = "HIV Diagnoses by Race Over Time (2011-2015)",
       color = NULL)+
  theme_stata()
```

```
## `summarise()` has grouped output by 'race'. You can override using the
## `.groups` argument.
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-42-1.png)<!-- -->


```r
data_clean %>% 
  group_by(race, year) %>% 
  filter(race != "All") %>% 
  summarize(sum_hiv_diagnoses = sum(hiv_diagnoses)) %>% 
  ggplot(aes(x = year, y = sum_hiv_diagnoses, fill = race))+
  geom_col()+
  labs(x = "Year",
       y = "HIV Diagnoses",
       title = "HIV Diagnoses by Race Over Time (2011-2015)",
       fill = NULL)+
  theme_stata()
```

```
## `summarise()` has grouped output by 'race'. You can override using the
## `.groups` argument.
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-43-1.png)<!-- -->


AIDS Diagnoses vs. Race: Are AIDS diagnoses more or less common across different races?

```r
data_clean %>% 
  filter(race != "All") %>% 
  group_by(race) %>% 
  summarize(sum_aids_diagnoses = sum(aids_diagnoses, na.rm = T))
```

```
## # A tibble: 5 × 2
##   race                   sum_aids_diagnoses
##   <chr>                               <dbl>
## 1 Asian/Pacific Islander                210
## 2 Black                                4091
## 3 Latino/Hispanic                      2564
## 4 Other/Unknown                          42
## 5 White                                 949
```


```r
data_clean %>% 
  filter(race != "All") %>% 
  group_by(race) %>% 
  summarize(sum_aids_diagnoses = sum(aids_diagnoses, na.rm = T)) %>% 
  ggplot(aes(x = race, y = sum_aids_diagnoses, fill = race))+
  geom_col()+
  labs(x = "Race",
       y = "AIDS Diagnoses",
       title = "AIDS Diagnoses by Race (2011-2015)")+
  theme_stata()+
  theme(legend.position = "none")
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-45-1.png)<!-- -->

AIDS Diagnoses vs. Race Over Time: Are AIDS diagnoses more or less common across different races over time?

Absolute

```r
data_clean %>% 
  filter(race != "All") %>% 
  filter(year == 2011 | year == 2015) %>% 
  group_by(race, year) %>% 
  summarize(sum_aids_diagnoses = sum(aids_diagnoses))
```

```
## `summarise()` has grouped output by 'race'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 10 × 3
## # Groups:   race [5]
##    race                    year sum_aids_diagnoses
##    <chr>                  <dbl>              <dbl>
##  1 Asian/Pacific Islander  2011                 41
##  2 Asian/Pacific Islander  2015                 47
##  3 Black                   2011               1097
##  4 Black                   2015                574
##  5 Latino/Hispanic         2011                681
##  6 Latino/Hispanic         2015                363
##  7 Other/Unknown           2011                  3
##  8 Other/Unknown           2015                 10
##  9 White                   2011                226
## 10 White                   2015                132
```


```r
data_clean %>% 
  filter(race != "All") %>% 
  filter(year == 2011 | year == 2015) %>% 
  group_by(race, year) %>% 
  summarize(sum_aids_diagnoses = sum(aids_diagnoses)) %>% 
  ggplot(aes(x = race, y = sum_aids_diagnoses, fill = as.factor(year)))+
  geom_col(position = "dodge")+
  labs(x = "Race",
       y = "AIDS Diagnoses",
       title = "AIDS Diagnoses by Race (2011-2015)",
       fill = "Year")+
  theme_stata()+
  scale_fill_brewer(palette = "Paired")
```

```
## `summarise()` has grouped output by 'race'. You can override using the
## `.groups` argument.
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-47-1.png)<!-- -->

Over Time

```r
data_clean %>% 
  filter(race != "All") %>% 
  group_by(race, year) %>% 
  summarize(sum_aids_diagnoses = sum(aids_diagnoses, na.rm = T))
```

```
## `summarise()` has grouped output by 'race'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 25 × 3
## # Groups:   race [5]
##    race                    year sum_aids_diagnoses
##    <chr>                  <dbl>              <dbl>
##  1 Asian/Pacific Islander  2011                 41
##  2 Asian/Pacific Islander  2012                 41
##  3 Asian/Pacific Islander  2013                 42
##  4 Asian/Pacific Islander  2014                 39
##  5 Asian/Pacific Islander  2015                 47
##  6 Black                   2011               1097
##  7 Black                   2012                945
##  8 Black                   2013                835
##  9 Black                   2014                640
## 10 Black                   2015                574
## # … with 15 more rows
```


```r
data_clean %>% 
  filter(race != "All") %>% 
  group_by(race, year) %>% 
  summarize(sum_aids_diagnoses = sum(aids_diagnoses, na.rm = T)) %>% 
  ggplot(aes(x = year, y = sum_aids_diagnoses, color = race))+
  geom_line()+
  labs(x = "Year",
       y = "AIDS Diagnoses",
       title = "AIDS Diagnoses by Race (2011-2015)",
       color = NULL)+
  theme_stata()
```

```
## `summarise()` has grouped output by 'race'. You can override using the
## `.groups` argument.
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-49-1.png)<!-- -->


```r
data_clean %>% 
  filter(race != "All") %>% 
  group_by(race, year) %>% 
  summarize(sum_aids_diagnoses = sum(aids_diagnoses, na.rm = T)) %>% 
  ggplot(aes(x = year, y = sum_aids_diagnoses, fill = race))+
  geom_col()+
  labs(x = "Year",
       y = "AIDS Diagnoses",
       title = "AIDS Diagnoses by Race (2011-2015)",
       fill = NULL)+
  theme_stata()
```

```
## `summarise()` has grouped output by 'race'. You can override using the
## `.groups` argument.
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-50-1.png)<!-- -->

HIV-Related Death Rate vs. Race: Does HIV-related death disproportionally affect different ethnicities?

```r
data_clean %>% 
  group_by(race) %>% 
  filter(race != "All") %>% 
  summarize(avg_hiv_death = mean(hiv_related_death_rate, na.rm = T))
```

```
## # A tibble: 5 × 2
##   race                   avg_hiv_death
##   <chr>                          <dbl>
## 1 Asian/Pacific Islander          2.89
## 2 Black                           4.61
## 3 Latino/Hispanic                 5.01
## 4 Other/Unknown                   1.31
## 5 White                           4.19
```


```r
data_clean %>% 
  group_by(race) %>% 
  filter(race != "All") %>% 
  summarize(avg_hiv_death = mean(hiv_related_death_rate, na.rm = T)) %>% 
  ggplot(aes(x = reorder(race, avg_hiv_death), y = avg_hiv_death, fill = race))+
  geom_col()+
  labs(x = "Race",
       y = "Average HIV-Related Death Rate",
       title = "HIV-Related Death Rate by Race (2011-2015)")+
  theme_stata()+
  theme(legend.position = "none")
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-52-1.png)<!-- -->

HIV-Related Death Rate vs. Race Over Time: Does HIV-related death disproportionally affect different ethnicities over time?

Absolute

```r
data_clean %>% 
  filter(race != "All") %>% 
  filter(year == 2011 | year == 2014) %>% 
  group_by(race, year) %>% 
  summarize(avg_hiv_related_death_rate = mean(hiv_related_death_rate))
```

```
## `summarise()` has grouped output by 'race'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 10 × 3
## # Groups:   race [5]
##    race                    year avg_hiv_related_death_rate
##    <chr>                  <dbl>                      <dbl>
##  1 Asian/Pacific Islander  2011                      4.23 
##  2 Asian/Pacific Islander  2014                      0.470
##  3 Black                   2011                      6.51 
##  4 Black                   2014                      4.09 
##  5 Latino/Hispanic         2011                      6.70 
##  6 Latino/Hispanic         2014                      3.41 
##  7 Other/Unknown           2011                      0.517
##  8 Other/Unknown           2014                      0.295
##  9 White                   2011                      4.99 
## 10 White                   2014                      4.35
```


```r
data_clean %>% 
  filter(race != "All") %>% 
  filter(year == 2011 | year == 2014) %>% 
  group_by(race, year) %>% 
  summarize(avg_hiv_related_death_rate = mean(hiv_related_death_rate)) %>% 
  ggplot(aes(x = race, y = avg_hiv_related_death_rate, fill = as.factor(year)))+
  geom_col(position = "dodge")+
  labs(x = "Race",
       y = "Average HIV-Related Death Rate (%)",
       title = "HIV-Related Death Rate by Race (2011-2014)",
       fill = "Year")+
  theme_stata()+
  scale_fill_brewer(palette = "Paired")
```

```
## `summarise()` has grouped output by 'race'. You can override using the
## `.groups` argument.
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-54-1.png)<!-- -->

Over Time

```r
data_clean %>% 
  group_by(race, year) %>% 
  filter(race != "All") %>% 
  summarize(avg_hiv_death_rate = mean(hiv_related_death_rate, na.rm = T))
```

```
## `summarise()` has grouped output by 'race'. You can override using the
## `.groups` argument.
```

```
## # A tibble: 25 × 3
## # Groups:   race [5]
##    race                    year avg_hiv_death_rate
##    <chr>                  <dbl>              <dbl>
##  1 Asian/Pacific Islander  2011              4.23 
##  2 Asian/Pacific Islander  2012              5.56 
##  3 Asian/Pacific Islander  2013              1.31 
##  4 Asian/Pacific Islander  2014              0.470
##  5 Asian/Pacific Islander  2015            NaN    
##  6 Black                   2011              6.51 
##  7 Black                   2012              4.42 
##  8 Black                   2013              3.41 
##  9 Black                   2014              4.09 
## 10 Black                   2015            NaN    
## # … with 15 more rows
```


```r
data_clean %>% 
  group_by(race, year) %>% 
  filter(race != "All") %>% 
  summarize(avg_hiv_death_rate = mean(hiv_related_death_rate, na.rm = T)) %>% 
  filter(year != 2015) %>% 
  ggplot(aes(x = year, y = avg_hiv_death_rate, color = race))+
  geom_line()+
  labs(x = "Year",
       y = "Average HIV-Related Death Rate",
       title = "HIV-Related Death Rate by Race (2011-2014)",
       color = NULL)+
  theme(plot.title = element_text(hjust = 0.5))+
  theme_stata()
```

```
## `summarise()` has grouped output by 'race'. You can override using the
## `.groups` argument.
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-56-1.png)<!-- -->

NYC Census Comparison

```r
data_clean %>% 
  group_by(race, year) %>% 
  filter(race != "All") %>% 
  summarize(sum_hiv_diagnoses = sum(hiv_diagnoses)) %>% 
  ggplot(aes(x = "", y = sum_hiv_diagnoses, fill = race))+
  geom_col()+
  coord_polar(theta = "y")+
  facet_wrap(~year, nrow = 1)+
  labs(title = "HIV Diagnoses by Race (2011-2015)",
       fill = NULL,
       x = "HIV Diagnoses",
       y = NULL)+
  theme_stata()+
  theme(axis.text.x = element_blank())
```

```
## `summarise()` has grouped output by 'race'. You can override using the
## `.groups` argument.
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-57-1.png)<!-- -->

```r
data_clean %>% 
  group_by(race, year) %>% 
  filter(race != "All") %>% 
  summarize(sum_aids_diagnoses = sum(aids_diagnoses)) %>% 
  ggplot(aes(x = "", y = sum_aids_diagnoses, fill = race))+
  geom_col()+
  coord_polar(theta = "y")+
  facet_wrap(~year, nrow = 1)+
  labs(title = "AIDS Diagnoses by Race (2011-2015)",
       fill = NULL,
       y = NULL,
       x = "AIDS Diagnoses")+
  theme_stata()+
  theme(axis.text.x = element_blank())
```

```
## `summarise()` has grouped output by 'race'. You can override using the
## `.groups` argument.
```

```
## Warning: Removed 1 rows containing missing values (position_stack).
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-58-1.png)<!-- -->



```r
demo_race_all %>% 
  ggplot(aes(x = "", y = percent_pop_for_year, fill = race))+
  geom_col()+
  coord_polar(theta = "y")+
  facet_wrap(~year, nrow = 1)+
  scale_fill_brewer(palette = "Set2", direction = 1)+
  labs(title = "NYC Population by Race (2011-2015)",
       fill = NULL,
       y = NULL,
       x = "Population")+
  theme_stata()+
  theme(axis.text.x = element_blank(),
        legend.text = element_text(size = 10))+
  guides(fill = guide_legend(nrow = 4))
```

![](BIS15L_Project_Benjamin_files/figure-html/unnamed-chunk-59-1.png)<!-- -->

