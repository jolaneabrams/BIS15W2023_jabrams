---
title: "Midterm 1"
author: "Jolane Abrams"
date: "2023-01-31"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Remember, you must remove the `#` for any included code chunks to run. Be sure to add your name to the author header above.  

After the first 50 minutes, please upload your code (5 points). During the second 50 minutes, you may get help from each other- but no copy/paste. Upload the last version at the end of this time, but be sure to indicate it as final. If you finish early, you are free to leave.

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean! Use the tidyverse and pipes unless otherwise indicated. This exam is worth a total of 35 points. 

Please load the following libraries.

```r
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
## ✔ ggplot2 3.4.0      ✔ purrr   1.0.0 
## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
## ✔ tidyr   1.2.1      ✔ stringr 1.5.0 
## ✔ readr   2.1.3      ✔ forcats 0.5.2 
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
library(janitor)
```

```
## 
## Attaching package: 'janitor'
## 
## The following objects are masked from 'package:stats':
## 
##     chisq.test, fisher.test
```

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ecs21351-sup-0003-SupplementS1.csv`. These data are from Soykan, C. U., J. Sauer, J. G. Schuetz, G. S. LeBaron, K. Dale, and G. M. Langham. 2016. Population trends for North American winter birds based on hierarchical models. Ecosphere 7(5):e01351. 10.1002/ecs2.1351.  

Please load these data as a new object called `ecosphere`. In this step, I am providing the code to load the data, clean the variable names, and remove a footer that the authors used as part of the original publication.

```r
ecosphere <- read_csv("data/ecs21351-sup-0003-SupplementS1.csv", skip=2) %>% 
  clean_names() %>%
  slice(1:(n() - 18)) # this removes the footer
```

Problem 1. (1 point) Let's start with some data exploration. What are the variable names?

```r
names(ecosphere)
```

```
##  [1] "order"                       "family"                     
##  [3] "common_name"                 "scientific_name"            
##  [5] "diet"                        "life_expectancy"            
##  [7] "habitat"                     "urban_affiliate"            
##  [9] "migratory_strategy"          "log10_mass"                 
## [11] "mean_eggs_per_clutch"        "mean_age_at_sexual_maturity"
## [13] "population_size"             "winter_range_area"          
## [15] "range_in_cbc"                "strata"                     
## [17] "circles"                     "feeder_bird"                
## [19] "median_trend"                "lower_95_percent_ci"        
## [21] "upper_95_percent_ci"
```

Problem 2. (1 point) Use the function of your choice to summarize the data.

```r
glimpse(ecosphere)
```

```
## Rows: 551
## Columns: 21
## $ order                       <chr> "Anseriformes", "Anseriformes", "Anserifor…
## $ family                      <chr> "Anatidae", "Anatidae", "Anatidae", "Anati…
## $ common_name                 <chr> "American Black Duck", "American Wigeon", …
## $ scientific_name             <chr> "Anas rubripes", "Anas americana", "Buceph…
## $ diet                        <chr> "Vegetation", "Vegetation", "Invertebrates…
## $ life_expectancy             <chr> "Long", "Middle", "Middle", "Long", "Middl…
## $ habitat                     <chr> "Wetland", "Wetland", "Wetland", "Wetland"…
## $ urban_affiliate             <chr> "No", "No", "No", "No", "No", "No", "No", …
## $ migratory_strategy          <chr> "Short", "Short", "Moderate", "Moderate", …
## $ log10_mass                  <dbl> 3.09, 2.88, 2.96, 3.11, 3.02, 2.88, 2.56, …
## $ mean_eggs_per_clutch        <dbl> 9.0, 7.5, 10.5, 3.5, 9.5, 13.5, 10.0, 8.5,…
## $ mean_age_at_sexual_maturity <dbl> 1.0, 1.0, 3.0, 2.5, 2.0, 1.0, 0.6, 2.0, 1.…
## $ population_size             <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ winter_range_area           <dbl> 3212473, 7145842, 1812841, 360134, 854350,…
## $ range_in_cbc                <dbl> 99.1, 61.7, 69.8, 53.7, 5.3, 0.5, 17.9, 72…
## $ strata                      <dbl> 82, 124, 37, 19, 36, 5, 26, 134, 145, 103,…
## $ circles                     <dbl> 1453, 1951, 502, 247, 470, 97, 479, 2189, …
## $ feeder_bird                 <chr> "No", "No", "No", "No", "No", "No", "No", …
## $ median_trend                <dbl> 1.014, 0.996, 1.039, 0.998, 1.004, 1.196, …
## $ lower_95_percent_ci         <dbl> 0.971, 0.964, 1.016, 0.956, 0.975, 1.152, …
## $ upper_95_percent_ci         <dbl> 1.055, 1.009, 1.104, 1.041, 1.036, 1.243, …
```

```r
anyNA(ecosphere)
```

```
## [1] TRUE
```

Problem 3. (2 points) How many distinct orders of birds are represented in the data?

```r
ecosphere %>% 
  summarize(n_bird_gen=n_distinct(order))
```

```
## # A tibble: 1 × 1
##   n_bird_gen
##        <int>
## 1         19
```

Problem 4. (2 points) Which habitat has the highest diversity (number of species) in the data?

```r
 ecosphere %>% 
  filter(habitat =="Grassland" | habitat =="Wetland"| habitat =="Woodland" |habitat == "Ocean" | habitat =="Shrubland") %>% 
  count(habitat, scientific_name)
```

```
## # A tibble: 492 × 3
##    habitat   scientific_name          n
##    <chr>     <chr>                <int>
##  1 Grassland Alauda arvensis          1
##  2 Grassland Ammodramus bairdii       1
##  3 Grassland Ammodramus henslowii     1
##  4 Grassland Anthus rubescens         1
##  5 Grassland Anthus spragueii         1
##  6 Grassland Asio flammeus            1
##  7 Grassland Athene cunicularia       1
##  8 Grassland Bubo scandiacus          1
##  9 Grassland Buteo albicaudatus       1
## 10 Grassland Buteo lagopus            1
## # … with 482 more rows
```

Run the code below to learn about the `slice` function. Look specifically at the examples (at the bottom) for `slice_max()` and `slice_min()`. If you are still unsure, try looking up examples online (https://rpubs.com/techanswers88/dplyr-slice). Use this new function to answer question 5 below.

```r
?slice_max
```

Problem 5. (4 points) Using the `slice_max()` or `slice_min()` function described above which species has the largest and smallest winter range?

```r
ecosphere %>% 
  slice_max(winter_range_area)
```

```
## # A tibble: 1 × 21
##   order     family commo…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶
##   <chr>     <chr>  <chr>   <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>
## 1 Procella… Proce… Sooty … Puffin… Vert… Long    Ocean   No      Long        2.9
## # … with 11 more variables: mean_eggs_per_clutch <dbl>,
## #   mean_age_at_sexual_maturity <dbl>, population_size <dbl>,
## #   winter_range_area <dbl>, range_in_cbc <dbl>, strata <dbl>, circles <dbl>,
## #   feeder_bird <chr>, median_trend <dbl>, lower_95_percent_ci <dbl>,
## #   upper_95_percent_ci <dbl>, and abbreviated variable names ¹​common_name,
## #   ²​scientific_name, ³​life_expectancy, ⁴​urban_affiliate, ⁵​migratory_strategy,
## #   ⁶​log10_mass
```


```r
ecosphere %>% 
slice_min(winter_range_area)
```

```
## # A tibble: 1 × 21
##   order     family commo…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶
##   <chr>     <chr>  <chr>   <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>
## 1 Passerif… Alaud… Skylark Alauda… Seed  Short   Grassl… No      Reside…    1.57
## # … with 11 more variables: mean_eggs_per_clutch <dbl>,
## #   mean_age_at_sexual_maturity <dbl>, population_size <dbl>,
## #   winter_range_area <dbl>, range_in_cbc <dbl>, strata <dbl>, circles <dbl>,
## #   feeder_bird <chr>, median_trend <dbl>, lower_95_percent_ci <dbl>,
## #   upper_95_percent_ci <dbl>, and abbreviated variable names ¹​common_name,
## #   ²​scientific_name, ³​life_expectancy, ⁴​urban_affiliate, ⁵​migratory_strategy,
## #   ⁶​log10_mass
```

Problem 6. (2 points) The family Anatidae includes ducks, geese, and swans. Make a new object `ducks` that only includes species in the family Anatidae. Restrict this new dataframe to include all variables except order and family.

```r
  ducks <- ecosphere %>% 
  filter(family == "Anatidae") %>% 
  select(!order, !family) 
  ducks
```

```
## # A tibble: 44 × 21
##    family  commo…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶ mean_…⁷
##    <chr>   <chr>   <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>   <dbl>
##  1 Anatid… "Ameri… Anas r… Vege… Long    Wetland No      Short      3.09     9  
##  2 Anatid… "Ameri… Anas a… Vege… Middle  Wetland No      Short      2.88     7.5
##  3 Anatid… "Barro… Buceph… Inve… Middle  Wetland No      Modera…    2.96    10.5
##  4 Anatid… "Black… Branta… Vege… Long    Wetland No      Modera…    3.11     3.5
##  5 Anatid… "Black… Melani… Inve… Middle  Wetland No      Modera…    3.02     9.5
##  6 Anatid… "Black… Dendro… Vege… Short   Wetland No      Withdr…    2.88    13.5
##  7 Anatid… "Blue-… Anas d… Vege… Middle  Wetland No      Modera…    2.56    10  
##  8 Anatid… "Buffl… Buceph… Inve… Middle  Wetland No      Short      2.6      8.5
##  9 Anatid… "Cackl… Branta… Vege… Middle  Wetland Yes     Short      3.45     5  
## 10 Anatid… "Canva… Aythya… Vege… Middle  Wetland No      Short      3.08     8  
## # … with 34 more rows, 11 more variables: mean_age_at_sexual_maturity <dbl>,
## #   population_size <dbl>, winter_range_area <dbl>, range_in_cbc <dbl>,
## #   strata <dbl>, circles <dbl>, feeder_bird <chr>, median_trend <dbl>,
## #   lower_95_percent_ci <dbl>, upper_95_percent_ci <dbl>, order <chr>, and
## #   abbreviated variable names ¹​common_name, ²​scientific_name,
## #   ³​life_expectancy, ⁴​urban_affiliate, ⁵​migratory_strategy, ⁶​log10_mass,
## #   ⁷​mean_eggs_per_clutch
```

Problem 7. (2 points) We might assume that all ducks live in wetland habitat. Is this true for the ducks in these data? If there are exceptions, list the species below.

```r
ducks %>% 
  filter (habitat != "Wetland") 
```

```
## # A tibble: 1 × 21
##   family   commo…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶ mean_…⁷
##   <chr>    <chr>   <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>   <dbl>
## 1 Anatidae Common… Somate… Inve… Middle  Ocean   No      Short      3.31       5
## # … with 11 more variables: mean_age_at_sexual_maturity <dbl>,
## #   population_size <dbl>, winter_range_area <dbl>, range_in_cbc <dbl>,
## #   strata <dbl>, circles <dbl>, feeder_bird <chr>, median_trend <dbl>,
## #   lower_95_percent_ci <dbl>, upper_95_percent_ci <dbl>, order <chr>, and
## #   abbreviated variable names ¹​common_name, ²​scientific_name,
## #   ³​life_expectancy, ⁴​urban_affiliate, ⁵​migratory_strategy, ⁶​log10_mass,
## #   ⁷​mean_eggs_per_clutch
```

Problem 8. (4 points) In ducks, how is mean body mass associated with migratory strategy? Do the ducks that migrate long distances have high or low average body mass?

```r
summary(ducks)
```

```
##     family          common_name        scientific_name        diet          
##  Length:44          Length:44          Length:44          Length:44         
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  life_expectancy      habitat          urban_affiliate    migratory_strategy
##  Length:44          Length:44          Length:44          Length:44         
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##    log10_mass    mean_eggs_per_clutch mean_age_at_sexual_maturity
##  Min.   :2.530   Min.   : 3.500       Min.   :0.40               
##  1st Qu.:2.873   1st Qu.: 6.250       1st Qu.:1.00               
##  Median :2.970   Median : 8.500       Median :1.25               
##  Mean   :3.043   Mean   : 8.273       Mean   :1.73               
##  3rd Qu.:3.160   3rd Qu.:10.000       3rd Qu.:2.50               
##  Max.   :4.040   Max.   :14.500       Max.   :5.50               
##                                                                  
##  population_size winter_range_area   range_in_cbc        strata     
##  Min.   : NA     Min.   :  212068   Min.   : 0.000   Min.   :  2.0  
##  1st Qu.: NA     1st Qu.:  812885   1st Qu.: 7.375   1st Qu.: 17.5  
##  Median : NA     Median : 4469242   Median :59.950   Median : 41.0  
##  Mean   :NaN     Mean   : 4284205   Mean   :49.041   Mean   : 65.3  
##  3rd Qu.: NA     3rd Qu.: 6748284   3rd Qu.:78.575   3rd Qu.:119.8  
##  Max.   : NA     Max.   :14379684   Max.   :99.100   Max.   :156.0  
##  NA's   :44                                                         
##     circles       feeder_bird         median_trend    lower_95_percent_ci
##  Min.   :  11.0   Length:44          Min.   :0.9120   Min.   :0.8210     
##  1st Qu.: 259.0   Class :character   1st Qu.:0.9975   1st Qu.:0.9640     
##  Median : 670.5   Mode  :character   Median :1.0190   Median :0.9955     
##  Mean   :1064.1                      Mean   :1.0214   Mean   :0.9900     
##  3rd Qu.:1951.0                      3rd Qu.:1.0350   3rd Qu.:1.0212     
##  Max.   :3006.0                      Max.   :1.1960   Max.   :1.1520     
##                                                                          
##  upper_95_percent_ci    order          
##  Min.   :0.969       Length:44         
##  1st Qu.:1.020       Class :character  
##  Median :1.042       Mode  :character  
##  Mean   :1.051                         
##  3rd Qu.:1.061                         
##  Max.   :1.243                         
## 
```

```r
filter(ducks, migratory_strategy == "Long")
```

```
## # A tibble: 2 × 21
##   family   commo…¹ scien…² diet  life_…³ habitat urban…⁴ migra…⁵ log10…⁶ mean_…⁷
##   <chr>    <chr>   <chr>   <chr> <chr>   <chr>   <chr>   <chr>     <dbl>   <dbl>
## 1 Anatidae Eurasi… Anas p… Vege… Long    Wetland No      Long       2.89     8.5
## 2 Anatidae Tufted… Aythya… Vege… Middle  Wetland Yes     Long       2.85    10  
## # … with 11 more variables: mean_age_at_sexual_maturity <dbl>,
## #   population_size <dbl>, winter_range_area <dbl>, range_in_cbc <dbl>,
## #   strata <dbl>, circles <dbl>, feeder_bird <chr>, median_trend <dbl>,
## #   lower_95_percent_ci <dbl>, upper_95_percent_ci <dbl>, order <chr>, and
## #   abbreviated variable names ¹​common_name, ²​scientific_name,
## #   ³​life_expectancy, ⁴​urban_affiliate, ⁵​migratory_strategy, ⁶​log10_mass,
## #   ⁷​mean_eggs_per_clutch
```

Problem 9. (2 points) Accipitridae is the family that includes eagles, hawks, kites, and osprey. First, make a new object `eagles` that only includes species in the family Accipitridae. Next, restrict these data to only include the variables common_name, scientific_name, and population_size.

```r
eagles <- 
filter (ecosphere, family == "Accipitridae") %>% 
select(common_name, scientific_name, population_size)
eagles
```

```
## # A tibble: 20 × 3
##    common_name         scientific_name          population_size
##    <chr>               <chr>                              <dbl>
##  1 Bald Eagle          Haliaeetus leucocephalus              NA
##  2 Broad-winged Hawk   Buteo platypterus                1700000
##  3 Cooper's Hawk       Accipiter cooperii                700000
##  4 Ferruginous Hawk    Buteo regalis                      80000
##  5 Golden Eagle        Aquila chrysaetos                 130000
##  6 Gray Hawk           Buteo nitidus                         NA
##  7 Harris's Hawk       Parabuteo unicinctus               50000
##  8 Hook-billed Kite    Chondrohierax uncinatus               NA
##  9 Northern Goshawk    Accipiter gentilis                200000
## 10 Northern Harrier    Circus cyaneus                    700000
## 11 Red-shouldered Hawk Buteo lineatus                   1100000
## 12 Red-tailed Hawk     Buteo jamaicensis                2000000
## 13 Rough-legged Hawk   Buteo lagopus                     300000
## 14 Sharp-shinned Hawk  Accipiter striatus                500000
## 15 Short-tailed Hawk   Buteo brachyurus                      NA
## 16 Snail Kite          Rostrhamus sociabilis                 NA
## 17 Swainson's Hawk     Buteo swainsoni                   540000
## 18 White-tailed Hawk   Buteo albicaudatus                    NA
## 19 White-tailed Kite   Elanus leucurus                       NA
## 20 Zone-tailed Hawk    Buteo albonotatus                     NA
```

Problem 10. (4 points) In the eagles data, any species with a population size less than 250,000 individuals is threatened. Make a new column `conservation_status` that shows whether or not a species is threatened.

```r
eagles %>% 
  mutate(conservation_status = population_size<=250000)
```

```
## # A tibble: 20 × 4
##    common_name         scientific_name          population_size conservation_s…¹
##    <chr>               <chr>                              <dbl> <lgl>           
##  1 Bald Eagle          Haliaeetus leucocephalus              NA NA              
##  2 Broad-winged Hawk   Buteo platypterus                1700000 FALSE           
##  3 Cooper's Hawk       Accipiter cooperii                700000 FALSE           
##  4 Ferruginous Hawk    Buteo regalis                      80000 TRUE            
##  5 Golden Eagle        Aquila chrysaetos                 130000 TRUE            
##  6 Gray Hawk           Buteo nitidus                         NA NA              
##  7 Harris's Hawk       Parabuteo unicinctus               50000 TRUE            
##  8 Hook-billed Kite    Chondrohierax uncinatus               NA NA              
##  9 Northern Goshawk    Accipiter gentilis                200000 TRUE            
## 10 Northern Harrier    Circus cyaneus                    700000 FALSE           
## 11 Red-shouldered Hawk Buteo lineatus                   1100000 FALSE           
## 12 Red-tailed Hawk     Buteo jamaicensis                2000000 FALSE           
## 13 Rough-legged Hawk   Buteo lagopus                     300000 FALSE           
## 14 Sharp-shinned Hawk  Accipiter striatus                500000 FALSE           
## 15 Short-tailed Hawk   Buteo brachyurus                      NA NA              
## 16 Snail Kite          Rostrhamus sociabilis                 NA NA              
## 17 Swainson's Hawk     Buteo swainsoni                   540000 FALSE           
## 18 White-tailed Hawk   Buteo albicaudatus                    NA NA              
## 19 White-tailed Kite   Elanus leucurus                       NA NA              
## 20 Zone-tailed Hawk    Buteo albonotatus                     NA NA              
## # … with abbreviated variable name ¹​conservation_status
```

Problem 11. (2 points) Consider the results from questions 9 and 10. Are there any species for which their threatened status needs further study? How do you know?

```r
  eagles %>% 
  filter(population_size<=250000)
```

```
## # A tibble: 4 × 3
##   common_name      scientific_name      population_size
##   <chr>            <chr>                          <dbl>
## 1 Ferruginous Hawk Buteo regalis                  80000
## 2 Golden Eagle     Aquila chrysaetos             130000
## 3 Harris's Hawk    Parabuteo unicinctus           50000
## 4 Northern Goshawk Accipiter gentilis            200000
```

Problem 12. (4 points) Use the `ecosphere` data to perform one exploratory analysis of your choice. The analysis must have a minimum of three lines and two functions. You must also clearly state the question you are attempting to answer.

How is migratory strategy related to habitat and longevity?

```r
ecosphere %>% 
  group_by(migratory_strategy,life_expectancy, habitat) %>% 
  summarize(migratory_strategy, life_expectancy, habitat)
```

```
## `summarise()` has grouped output by 'migratory_strategy', 'life_expectancy',
## 'habitat'. You can override using the `.groups` argument.
```

```
## # A tibble: 551 × 3
## # Groups:   migratory_strategy, life_expectancy, habitat [76]
##    migratory_strategy life_expectancy habitat  
##    <chr>              <chr>           <chr>    
##  1 Irruptive          Long            Grassland
##  2 Irruptive          Middle          Grassland
##  3 Irruptive          Middle          Woodland 
##  4 Irruptive          Middle          Woodland 
##  5 Irruptive          Middle          Woodland 
##  6 Irruptive          Middle          Woodland 
##  7 Irruptive          Short           Shrubland
##  8 Irruptive          Short           Shrubland
##  9 Irruptive          Short           Various  
## 10 Irruptive          Short           Woodland 
## # … with 541 more rows
```



Please provide the names of the students you have worked with with during the exam:
Kushleen

Please be 100% sure your exam is saved, knitted, and pushed to your github repository. No need to submit a link on canvas, we will find your exam in your repository.
