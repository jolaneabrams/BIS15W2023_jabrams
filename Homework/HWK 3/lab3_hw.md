---
title: "Lab 3 Homework"
author: "Jolane Abrams"
date: "2023-01-19"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Mammals Sleep
1. For this assignment, we are going to use built-in data on mammal sleep patterns. From which publication are these data taken from? Since the data are built-in you can use the help function in R.

```r
??mammals
```

```
## starting httpd help server ... done
```
mammals_sleep_allison_cicchetti_1976


2. Store these data into a new data frame `sleep`.

```r
sleep <- readr::read_csv("mammals_sleep_allison_cicchetti_1976.csv")
```

```
## Rows: 62 Columns: 11
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr  (1): species
## dbl (10): body weight in kg, brain weight in g, slow wave ("nondreaming") sl...
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

3. What are the dimensions of this data frame (variables and observations)? How do you know? Please show the *code* that you used to determine this below.  

```r
dim (sleep)
```

```
## [1] 62 11
```
62 observations of 11 variables - can see also in the environment window


4. Are there any NAs in the data? How did you determine this? Please show your code.  

```r
view(sleep)
```
Looks like all observations are there.


5. Show a list of the column names in this data frame.

```r
names(sleep) #Show column names
```

```
##  [1] "species"                                                        
##  [2] "body weight in kg"                                              
##  [3] "brain weight in g"                                              
##  [4] "slow wave (\"nondreaming\") sleep (hrs/day)"                    
##  [5] "paradoxical (\"dreaming\") sleep (hrs/day)"                     
##  [6] "total sleep (hrs/day)  (sum of slow wave and paradoxical sleep)"
##  [7] "maximum life span (years)"                                      
##  [8] "gestation time (days)"                                          
##  [9] "predation index (1-5)"                                          
## [10] "sleep exposure index (1-5)"                                     
## [11] "overall danger index (1-5)"
```

6. How many herbivores are represented in the data?  

```r
str(sleep) #Find out what variable names are
```

```
## spec_tbl_df [62 × 11] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ species                                                        : chr [1:62] "African elephant" "African giant pouched rat" "Arctic Fox" "Arctic ground squirrel" ...
##  $ body weight in kg                                              : num [1:62] 6654 1 3.38 0.92 2547 ...
##  $ brain weight in g                                              : num [1:62] 5712 6.6 44.5 5.7 4603 ...
##  $ slow wave ("nondreaming") sleep (hrs/day)                      : num [1:62] -999 6.3 -999 -999 2.1 9.1 15.8 5.2 10.9 8.3 ...
##  $ paradoxical ("dreaming") sleep (hrs/day)                       : num [1:62] -999 2 -999 -999 1.8 0.7 3.9 1 3.6 1.4 ...
##  $ total sleep (hrs/day)  (sum of slow wave and paradoxical sleep): num [1:62] 3.3 8.3 12.5 16.5 3.9 9.8 19.7 6.2 14.5 9.7 ...
##  $ maximum life span (years)                                      : num [1:62] 38.6 4.5 14 -999 69 27 19 30.4 28 50 ...
##  $ gestation time (days)                                          : num [1:62] 645 42 60 25 624 180 35 392 63 230 ...
##  $ predation index (1-5)                                          : num [1:62] 3 3 1 5 3 4 1 4 1 1 ...
##  $ sleep exposure index (1-5)                                     : num [1:62] 5 1 1 2 5 4 1 5 2 1 ...
##  $ overall danger index (1-5)                                     : num [1:62] 3 3 1 3 4 4 1 4 1 1 ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   species = col_character(),
##   ..   `body weight in kg` = col_double(),
##   ..   `brain weight in g` = col_double(),
##   ..   `slow wave ("nondreaming") sleep (hrs/day)` = col_double(),
##   ..   `paradoxical ("dreaming") sleep (hrs/day)` = col_double(),
##   ..   `total sleep (hrs/day)  (sum of slow wave and paradoxical sleep)` = col_double(),
##   ..   `maximum life span (years)` = col_double(),
##   ..   `gestation time (days)` = col_double(),
##   ..   `predation index (1-5)` = col_double(),
##   ..   `sleep exposure index (1-5)` = col_double(),
##   ..   `overall danger index (1-5)` = col_double()
##   .. )
##  - attr(*, "problems")=<externalptr>
```

```r
herbs <- filter(sleep,`predation index (1-5)`== 5) #Filtering out the herbivores, defined as those with predation index =5
herbs
```

```
## # A tibble: 14 × 11
##    species       body …¹ brain…² slow …³ parad…⁴ total…⁵ maxim…⁶ gesta…⁷ preda…⁸
##    <chr>           <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
##  1 Arctic groun…   0.92     5.7   -999    -999      16.5  -999      25         5
##  2 Chinchilla      0.425    6.4     11       1.5    12.5     7     112         5
##  3 Cow           465      423        3.2     0.7     3.9    30     281         5
##  4 Donkey        187.     419     -999    -999       3.1    40     365         5
##  5 Giraffe       529      680     -999       0.3  -999      28     400         5
##  6 Goat           27.7    115        3.3     0.5     3.8    20     148         5
##  7 Ground squir…   0.101    4       10.4     3.4    13.8     9      28         5
##  8 Guinea pig      1.04     5.5      7.4     0.8     8.2     7.6    68         5
##  9 Horse         521      655        2.1     0.8     2.9    46     336         5
## 10 Lesser short…   0.005    0.14     7.7     1.4     9.1     2.6    21.5       5
## 11 Okapi         250      490     -999       1    -999      23.6   440         5
## 12 Rabbit          2.5     12.1      7.5     0.9     8.4    18      31         5
## 13 Roe deer       14.8     98.2   -999    -999       2.6    17     150         5
## 14 Sheep          55.5    175        3.2     0.6     3.8    20     151         5
## # … with 2 more variables: `sleep exposure index (1-5)` <dbl>,
## #   `overall danger index (1-5)` <dbl>, and abbreviated variable names
## #   ¹​`body weight in kg`, ²​`brain weight in g`,
## #   ³​`slow wave ("nondreaming") sleep (hrs/day)`,
## #   ⁴​`paradoxical ("dreaming") sleep (hrs/day)`,
## #   ⁵​`total sleep (hrs/day)  (sum of slow wave and paradoxical sleep)`,
## #   ⁶​`maximum life span (years)`, ⁷​`gestation time (days)`, …
```
14 herbivores


7. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 1kg body weight and large as greater than or equal to 200kg body weight. Make two new dataframes (large and small) based on these parameters.

```r
small <- filter(sleep, `body weight in kg`<=1) #Make small dataframe
small
```

```
## # A tibble: 21 × 11
##    species       body …¹ brain…² slow …³ parad…⁴ total…⁵ maxim…⁶ gesta…⁷ preda…⁸
##    <chr>           <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
##  1 African gian…   1         6.6     6.3     2       8.3     4.5      42       3
##  2 Arctic groun…   0.92      5.7  -999    -999      16.5  -999        25       5
##  3 Big brown bat   0.023     0.3    15.8     3.9    19.7    19        35       1
##  4 Chinchilla      0.425     6.4    11       1.5    12.5     7       112       5
##  5 Desert hedge…   0.55      2.4     7.6     2.7    10.3  -999      -999       2
##  6 Eastern Amer…   0.075     1.2     6.3     2.1     8.4     3.5      42       1
##  7 European hed…   0.785     3.5     6.6     4.1    10.7     6        42       2
##  8 Galago          0.2       5       9.5     1.2    10.7    10.4     120       2
##  9 Golden hamst…   0.12      1      11       3.4    14.4     3.9      16       3
## 10 Ground squir…   0.101     4      10.4     3.4    13.8     9        28       5
## # … with 11 more rows, 2 more variables: `sleep exposure index (1-5)` <dbl>,
## #   `overall danger index (1-5)` <dbl>, and abbreviated variable names
## #   ¹​`body weight in kg`, ²​`brain weight in g`,
## #   ³​`slow wave ("nondreaming") sleep (hrs/day)`,
## #   ⁴​`paradoxical ("dreaming") sleep (hrs/day)`,
## #   ⁵​`total sleep (hrs/day)  (sum of slow wave and paradoxical sleep)`,
## #   ⁶​`maximum life span (years)`, ⁷​`gestation time (days)`, …
```

```r
large <- filter(sleep, `body weight in kg`>=200) #Make large dataframe
large
```

```
## # A tibble: 7 × 11
##   species        body …¹ brain…² slow …³ parad…⁴ total…⁵ maxim…⁶ gesta…⁷ preda…⁸
##   <chr>            <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
## 1 African eleph…    6654    5712  -999    -999       3.3    38.6     645       3
## 2 Asian elephant    2547    4603     2.1     1.8     3.9    69       624       3
## 3 Cow                465     423     3.2     0.7     3.9    30       281       5
## 4 Giraffe            529     680  -999       0.3  -999      28       400       5
## 5 Gorilla            207     406  -999    -999      12      39.3     252       1
## 6 Horse              521     655     2.1     0.8     2.9    46       336       5
## 7 Okapi              250     490  -999       1    -999      23.6     440       5
## # … with 2 more variables: `sleep exposure index (1-5)` <dbl>,
## #   `overall danger index (1-5)` <dbl>, and abbreviated variable names
## #   ¹​`body weight in kg`, ²​`brain weight in g`,
## #   ³​`slow wave ("nondreaming") sleep (hrs/day)`,
## #   ⁴​`paradoxical ("dreaming") sleep (hrs/day)`,
## #   ⁵​`total sleep (hrs/day)  (sum of slow wave and paradoxical sleep)`,
## #   ⁶​`maximum life span (years)`, ⁷​`gestation time (days)`, …
```

8. What is the mean weight for both the small and large mammals?

```r
summary(small) #Gives mean for small 
```

```
##    species          body weight in kg brain weight in g
##  Length:21          Min.   :0.0050    Min.   : 0.14    
##  Class :character   1st Qu.:0.0600    1st Qu.: 1.00    
##  Mode  :character   Median :0.1220    Median : 2.50    
##                     Mean   :0.3324    Mean   : 3.62    
##                     3rd Qu.:0.5500    3rd Qu.: 5.00    
##                     Max.   :1.0000    Max.   :15.50    
##  slow wave ("nondreaming") sleep (hrs/day)
##  Min.   :-999.00                          
##  1st Qu.:   7.60                          
##  Median :  10.40                          
##  Mean   : -37.82                          
##  3rd Qu.:  11.00                          
##  Max.   :  17.90                          
##  paradoxical ("dreaming") sleep (hrs/day)
##  Min.   :-999.00                         
##  1st Qu.:   1.50                         
##  Median :   2.10                         
##  Mean   : -45.39                         
##  3rd Qu.:   2.60                         
##  Max.   :   4.10                         
##  total sleep (hrs/day)  (sum of slow wave and paradoxical sleep)
##  Min.   : 6.60                                                  
##  1st Qu.:10.30                                                  
##  Median :12.80                                                  
##  Mean   :12.72                                                  
##  3rd Qu.:14.40                                                  
##  Max.   :19.90                                                  
##  maximum life span (years) gestation time (days) predation index (1-5)
##  Min.   :-999.0            Min.   :-999.00       Min.   :1.000        
##  1st Qu.:   2.6            1st Qu.:  21.50       1st Qu.:2.000        
##  Median :   4.5            Median :  35.00       Median :3.000        
##  Mean   :-136.6            Mean   : -42.55       Mean   :2.857        
##  3rd Qu.:   7.0            3rd Qu.:  50.00       3rd Qu.:4.000        
##  Max.   :  24.0            Max.   : 225.00       Max.   :5.000        
##  sleep exposure index (1-5) overall danger index (1-5)
##  Min.   :1.000              Min.   :1.000             
##  1st Qu.:1.000              1st Qu.:2.000             
##  Median :1.000              Median :2.000             
##  Mean   :1.476              Mean   :2.286             
##  3rd Qu.:2.000              3rd Qu.:3.000             
##  Max.   :4.000              Max.   :4.000
```
0.3324

```r
summary(large) #Gives mean for large
```

```
##    species          body weight in kg brain weight in g
##  Length:7           Min.   : 207.0    Min.   : 406.0   
##  Class :character   1st Qu.: 357.5    1st Qu.: 456.5   
##  Mode  :character   Median : 521.0    Median : 655.0   
##                     Mean   :1596.1    Mean   :1852.7   
##                     3rd Qu.:1538.0    3rd Qu.:2641.5   
##                     Max.   :6654.0    Max.   :5712.0   
##  slow wave ("nondreaming") sleep (hrs/day)
##  Min.   :-999.0                           
##  1st Qu.:-999.0                           
##  Median :-999.0                           
##  Mean   :-569.8                           
##  3rd Qu.:   2.1                           
##  Max.   :   3.2                           
##  paradoxical ("dreaming") sleep (hrs/day)
##  Min.   :-999.0                          
##  1st Qu.:-499.4                          
##  Median :   0.7                          
##  Mean   :-284.8                          
##  3rd Qu.:   0.9                          
##  Max.   :   1.8                          
##  total sleep (hrs/day)  (sum of slow wave and paradoxical sleep)
##  Min.   :-999.0                                                 
##  1st Qu.:-498.1                                                 
##  Median :   3.3                                                 
##  Mean   :-281.7                                                 
##  3rd Qu.:   3.9                                                 
##  Max.   :  12.0                                                 
##  maximum life span (years) gestation time (days) predation index (1-5)
##  Min.   :23.60             Min.   :252.0         Min.   :1.000        
##  1st Qu.:29.00             1st Qu.:308.5         1st Qu.:3.000        
##  Median :38.60             Median :400.0         Median :5.000        
##  Mean   :39.21             Mean   :425.4         Mean   :3.857        
##  3rd Qu.:42.65             3rd Qu.:532.0         3rd Qu.:5.000        
##  Max.   :69.00             Max.   :645.0         Max.   :5.000        
##  sleep exposure index (1-5) overall danger index (1-5)
##  Min.   :4.000              Min.   :1.0               
##  1st Qu.:5.000              1st Qu.:3.5               
##  Median :5.000              Median :5.0               
##  Mean   :4.857              Mean   :4.0               
##  3rd Qu.:5.000              3rd Qu.:5.0               
##  Max.   :5.000              Max.   :5.0
```
1596.1
9. Using a similar approach as above, do large or small animals sleep longer on average?  

Referring to the summaries given above, the small animals sleep an average of 12.72hrs/day while the large ones' mean sleep time is -281.7 (some data glitch there); the small animals tend to sleep more.


10. Which animal is the sleepiest among the entire dataframe?

```r
summary(sleep) #To see maximum sleep value
```

```
##    species          body weight in kg  brain weight in g
##  Length:62          Min.   :   0.005   Min.   :   0.14  
##  Class :character   1st Qu.:   0.600   1st Qu.:   4.25  
##  Mode  :character   Median :   3.342   Median :  17.25  
##                     Mean   : 198.790   Mean   : 283.13  
##                     3rd Qu.:  48.202   3rd Qu.: 166.00  
##                     Max.   :6654.000   Max.   :5712.00  
##  slow wave ("nondreaming") sleep (hrs/day)
##  Min.   :-999.000                         
##  1st Qu.:   2.375                         
##  Median :   7.400                         
##  Mean   :-218.866                         
##  3rd Qu.:  10.550                         
##  Max.   :  17.900                         
##  paradoxical ("dreaming") sleep (hrs/day)
##  Min.   :-999.000                        
##  1st Qu.:   0.500                        
##  Median :   1.300                        
##  Mean   :-191.764                        
##  3rd Qu.:   2.275                        
##  Max.   :   6.600                        
##  total sleep (hrs/day)  (sum of slow wave and paradoxical sleep)
##  Min.   :-999.00                                                
##  1st Qu.:   6.20                                                
##  Median :  10.30                                                
##  Mean   : -54.60                                                
##  3rd Qu.:  13.15                                                
##  Max.   :  19.90                                                
##  maximum life span (years) gestation time (days) predation index (1-5)
##  Min.   :-999.00           Min.   :-999.00       Min.   :1.000        
##  1st Qu.:   5.25           1st Qu.:  30.25       1st Qu.:2.000        
##  Median :  13.35           Median :  63.00       Median :3.000        
##  Mean   : -45.86           Mean   :  68.72       Mean   :2.871        
##  3rd Qu.:  27.00           3rd Qu.: 195.00       3rd Qu.:4.000        
##  Max.   : 100.00           Max.   : 645.00       Max.   :5.000        
##  sleep exposure index (1-5) overall danger index (1-5)
##  Min.   :1.000              Min.   :1.000             
##  1st Qu.:1.000              1st Qu.:1.000             
##  Median :2.000              Median :2.000             
##  Mean   :2.419              Mean   :2.613             
##  3rd Qu.:4.000              3rd Qu.:4.000             
##  Max.   :5.000              Max.   :5.000
```


```r
#Picks out the animal with the maximum sleep value
sleepiest <- filter(sleep,`total sleep (hrs/day)  (sum of slow wave and paradoxical sleep)`==19.90)
sleepiest
```

```
## # A tibble: 1 × 11
##   species        body …¹ brain…² slow …³ parad…⁴ total…⁵ maxim…⁶ gesta…⁷ preda…⁸
##   <chr>            <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>   <dbl>
## 1 Little brown …    0.01    0.25    17.9       2    19.9      24      50       1
## # … with 2 more variables: `sleep exposure index (1-5)` <dbl>,
## #   `overall danger index (1-5)` <dbl>, and abbreviated variable names
## #   ¹​`body weight in kg`, ²​`brain weight in g`,
## #   ³​`slow wave ("nondreaming") sleep (hrs/day)`,
## #   ⁴​`paradoxical ("dreaming") sleep (hrs/day)`,
## #   ⁵​`total sleep (hrs/day)  (sum of slow wave and paradoxical sleep)`,
## #   ⁶​`maximum life span (years)`, ⁷​`gestation time (days)`, …
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
