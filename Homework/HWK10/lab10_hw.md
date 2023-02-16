---
title: "Lab 10 Homework"
author: "Jolane Abrams"
date: "2023-02-15"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(naniar)
library(skimr)
```

## Desert Ecology
For this assignment, we are going to use a modified data set on [desert ecology](http://esapubs.org/archive/ecol/E090/118/). The data are from: S. K. Morgan Ernest, Thomas J. Valone, and James H. Brown. 2009. Long-term monitoring and experimental manipulation of a Chihuahuan Desert ecosystem near Portal, Arizona, USA. Ecology 90:1708.

```r
deserts <- read_csv(here("lab10", "data", "surveys_complete.csv"))
```

```
## Rows: 34786 Columns: 13
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (6): species_id, sex, genus, species, taxa, plot_type
## dbl (7): record_id, month, day, year, plot_id, hindfoot_length, weight
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. Use the function(s) of your choice to get an idea of its structure, including how NA's are treated. Are the data tidy?  

```r
glimpse(deserts)
```

```
## Rows: 34,786
## Columns: 13
## $ record_id       <dbl> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16,…
## $ month           <dbl> 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, 7, …
## $ day             <dbl> 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16, 16…
## $ year            <dbl> 1977, 1977, 1977, 1977, 1977, 1977, 1977, 1977, 1977, …
## $ plot_id         <dbl> 2, 3, 2, 7, 3, 1, 2, 1, 1, 6, 5, 7, 3, 8, 6, 4, 3, 2, …
## $ species_id      <chr> "NL", "NL", "DM", "DM", "DM", "PF", "PE", "DM", "DM", …
## $ sex             <chr> "M", "M", "F", "M", "M", "M", "F", "M", "F", "F", "F",…
## $ hindfoot_length <dbl> 32, 33, 37, 36, 35, 14, NA, 37, 34, 20, 53, 38, 35, NA…
## $ weight          <dbl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ genus           <chr> "Neotoma", "Neotoma", "Dipodomys", "Dipodomys", "Dipod…
## $ species         <chr> "albigula", "albigula", "merriami", "merriami", "merri…
## $ taxa            <chr> "Rodent", "Rodent", "Rodent", "Rodent", "Rodent", "Rod…
## $ plot_type       <chr> "Control", "Long-term Krat Exclosure", "Control", "Rod…
```

```r
deserts %>% 
naniar::miss_var_summary()
```

```
## # A tibble: 13 × 3
##    variable        n_miss pct_miss
##    <chr>            <int>    <dbl>
##  1 hindfoot_length   3348     9.62
##  2 weight            2503     7.20
##  3 sex               1748     5.03
##  4 record_id            0     0   
##  5 month                0     0   
##  6 day                  0     0   
##  7 year                 0     0   
##  8 plot_id              0     0   
##  9 species_id           0     0   
## 10 genus                0     0   
## 11 species              0     0   
## 12 taxa                 0     0   
## 13 plot_type            0     0
```

2. How many genera and species are represented in the data? What are the total number of observations? Which species is most/ least frequently sampled in the study?

```r
deserts %>% 
  summarize(n_genera=n_distinct(genus), 
            n_species=n_distinct(species),
            n_obs=n())
```

```
## # A tibble: 1 × 3
##   n_genera n_species n_obs
##      <int>     <int> <int>
## 1       26        40 34786
```

```r
deserts %>%
  group_by(species_id) %>%
  summarize(count = n()) %>%
  group_by(species_id) %>%
  summarize(count = sum(count)) %>%
  arrange(-count) %>%
  slice(c(1, n()))
```

```
## # A tibble: 2 × 2
##   species_id count
##   <chr>      <int>
## 1 DM         10596
## 2 ST             1
```

3. What is the proportion of taxa included in this study? Show a table and plot that reflects this count.


```r
deserts %>%
  summarize(
    bird_prop = nrow(filter(., taxa == "Bird")) / nrow(.),
    rabbit_prop = nrow(filter(., taxa == "Rabbit")) / nrow(.),
    reptile_prop = nrow(filter(., taxa == "Reptile")) / nrow(.),
    rodent_prop = nrow(filter(., taxa == "Rodent")) / nrow(.)
  )
```

```
## # A tibble: 1 × 4
##   bird_prop rabbit_prop reptile_prop rodent_prop
##       <dbl>       <dbl>        <dbl>       <dbl>
## 1    0.0129     0.00216     0.000402       0.985
```

```r
deserts %>%
  summarize(
    bird_prop = nrow(filter(., taxa == "Bird")) / nrow(.),
    rabbit_prop = nrow(filter(., taxa == "Rabbit")) / nrow(.),
    reptile_prop = nrow(filter(., taxa == "Reptile")) / nrow(.),
    rodent_prop = nrow(filter(., taxa == "Rodent")) / nrow(.)
  ) %>%
  gather(taxon, proportion) %>%
  ggplot(aes(x = taxon, fill=taxon, y = proportion)) +
  geom_col() +
  labs(x = "Taxon", y = "Proportion")
```

![](lab10_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

4. For the taxa included in the study, use the fill option to show the proportion of individuals sampled by `plot_type.`


```r
deserts %>% 
  ggplot(aes(x=taxa, fill=plot_type)) + geom_bar(position="dodge") +
  scale_y_log10()+
  theme(axis.text.x = element_text(hjust = 0.5)) +
  labs(title = "Included Taxa by plot type",
       x = NULL,
       y= " log scaled n")
```

![](lab10_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->


5. What is the range of weight for each species included in the study? Remove any observations of weight that are NA so they do not show up in the plot.

```r
deserts %>% 
  filter(weight!="NA") %>% 
  ggplot(aes(x=species_id, y=weight)) +
  geom_boxplot()+
  labs(title = "Distribution of weight for each species",
       x = "Species ID",
       y = "Weight")
```

![](lab10_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->


6. Add another layer to your answer from #4 using `geom_point` to get an idea of how many measurements were taken for each species.


```r
deserts %>% 
  filter(weight!="NA") %>% 
  ggplot(aes(x=species_id, y=weight)) +
  geom_boxplot()+
  geom_point(alpha=0.3, color="tomato", position = "jitter") +
  coord_flip()+
  labs(title = "Distribution of weight for each species",
       x = "Species ID",
       y = "Weight") 
```

![](lab10_hw_files/figure-html/unnamed-chunk-11-1.png)<!-- -->


7. [Dipodomys merriami](https://en.wikipedia.org/wiki/Merriam's_kangaroo_rat) is the most frequently sampled animal in the study. How have the number of observations of this species changed over the years included in the study?

```r
deserts %>% 
  filter(species_id=="DM") %>% 
  group_by(year) %>% 
  summarize(n_samples=n()) %>% 
  ggplot(aes(x=as.factor(year), y=n_samples)) + geom_col()+
  theme(axis.text.x = element_text(angle = 60, hjust = 1)) +
  labs(title = "Dipodomys merriami observations 1977-2002",
       x = NULL,
       y= "n")
```

![](lab10_hw_files/figure-html/unnamed-chunk-12-1.png)<!-- -->


8. What is the relationship between `weight` and `hindfoot` length? Consider whether or not over plotting is an issue.


```r
deserts %>% 
 filter(weight!="NA" & hindfoot_length!="NA" & species_id!="NA") %>% 
  select(species_id, weight, hindfoot_length) %>% 
  ggplot(aes(x=weight,fill=species_id, y=hindfoot_length)) +
  geom_boxplot()+
  labs(title = "Weight & Hindfoot Length",
       x = "Weight",
       y = "Hindfoot Length") 
```

![](lab10_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->


9. Which two species have, on average, the highest weight? Once you have identified them, make a new column that is a ratio of `weight` to `hindfoot_length`. Make a plot that shows the range of this new ratio and fill by sex.

```r
deserts %>% 
  filter(species_id=="NL" | species_id=="DS") %>% 
  filter(weight!="NA" & hindfoot_length!="NA" & sex!="NA") %>% 
  mutate(ratio=weight/hindfoot_length) %>% 
  select(species_id, sex, weight, hindfoot_length, ratio) %>% 
  ggplot(aes(x=species_id, y=ratio, fill=sex)) + geom_boxplot()+
  labs(title = "Range of Weight/ Hindfoot Length for species NL and DS",
       x = "Species ID",
       y = "Weight/ Hindfoot Length") 
```

![](lab10_hw_files/figure-html/unnamed-chunk-14-1.png)<!-- -->



10. Make one plot of your choice! Make sure to include at least two of the aesthetics options you have learned.


```r
deserts %>% 
  filter(weight != "NA" & sex != "NA") %>% 
  group_by(species_id, sex) %>%
  ggplot(aes(x = species_id, y = weight, fill = sex)) +
  geom_boxplot()
```

![](lab10_hw_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

```r
deserts %>% 
filter(species_id=="DO" | species_id=="PM"| species_id=="RF"|species_id=="RM"|species_id=="SF"|
         species_id=="SH"| species_id=="SO"|species_id=="PE"|species_id=="PM"| species_id=="PX"|species_id=="RF"|species_id=="RM"|species_id=="RO"|species_id=="OT") %>% 
  filter(weight!="NA" & sex!="NA") %>%
  select(species_id, sex, weight) %>% 
  ggplot(aes(x=species_id, y=weight, fill=sex)) + 
  geom_boxplot()+
  scale_y_log10()+
  labs(title = "Weight Ranges for Species with Females of Equal or Greater Weight",
       x = "Species ID",
       y = "log10 Weight") 
```

![](lab10_hw_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

```r
deserts %>% 
filter(species_id=="DO" | species_id=="PM"| species_id=="RF"|species_id=="RM"|species_id=="SF"|
         species_id=="SH"| species_id=="SO"|species_id=="PE"|species_id=="PM"| species_id=="PX"|species_id=="RF"|species_id=="RM"|species_id=="RO"|species_id=="OT") %>% 
  filter(weight!="NA" & sex!="NA") %>%
  select(species_id, sex, weight) %>% 
  ggplot(aes(x=species_id, y=weight, fill=sex)) + 
  geom_col(position = "dodge")+
  scale_y_log10()+
  labs(title = "Weight Ranges for Species with Females of Equal or Greater Weight",
       x = "Species ID",
       y = "log10 Weight")
```

![](lab10_hw_files/figure-html/unnamed-chunk-17-1.png)<!-- -->


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
