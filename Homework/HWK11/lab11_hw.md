---
title: "Lab 11 Homework"
author: "Jolane Abrams"
date: "2023-02-18"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

**In this homework, you should make use of the aesthetics you have learned. It's OK to be flashy!**

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(naniar)
library(ggthemes)
library(RColorBrewer)
library(paletteer)
```

## Resources
The idea for this assignment came from [Rebecca Barter's](http://www.rebeccabarter.com/blog/2017-11-17-ggplot2_tutorial/) ggplot tutorial so if you get stuck this is a good place to have a look.  

## Gapminder
For this assignment, we are going to use the dataset [gapminder](https://cran.r-project.org/web/packages/gapminder/index.html). Gapminder includes information about economics, population, and life expectancy from countries all over the world. You will need to install it before use. This is the same data that we will use for midterm 2 so this is good practice.

```r
library(gapminder)
```

## Questions
The questions below are open-ended and have many possible solutions. Your approach should, where appropriate, include numerical summaries and visuals. Be creative; assume you are building an analysis that you would ultimately present to an audience of stakeholders. Feel free to try out different `geoms` if they more clearly present your results.  

**1. Use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine how NA's are treated in the data.**  


```r
gapminder <- gapminder %>% 
  clean_names()
```


```r
names(gapminder)
```

```
## [1] "country"    "continent"  "year"       "life_exp"   "pop"       
## [6] "gdp_percap"
```



```r
options(scipen=999) #cancels the use of scientific notation for the session
```


**2. Among the interesting variables in gapminder is life expectancy. How has global life expectancy changed between 1952 and 2007?**


```r
gapminder%>% 
  group_by(year,life_exp) %>% 
  summarise(n=n(), .groups='keep')
```

```
## # A tibble: 1,700 × 3
## # Groups:   year, life_exp [1,700]
##     year life_exp     n
##    <int>    <dbl> <int>
##  1  1952     28.8     1
##  2  1952     30       1
##  3  1952     30.0     1
##  4  1952     30.3     1
##  5  1952     31.3     1
##  6  1952     32.0     1
##  7  1952     32.5     1
##  8  1952     32.5     1
##  9  1952     33.0     1
## 10  1952     33.6     1
## # … with 1,690 more rows
```

```r
gapminder2 <- gapminder %>% mutate(year=as_factor(year))
```


```r
gapminder2 %>% 
  group_by(year, life_exp) %>% 
  summarise(n=n(), .groups='keep') %>% 
  ggplot(aes(x=year, y=life_exp, fill=year))+
  geom_boxplot()+
  theme(axis.text.x = element_text(angle = 60, hjust = 1))+
  labs(title = "Changes in Life Expectancy, 1952 - 2007 ",
       x = "Year",
       fill = "n")
```

![](lab11_hw_files/figure-html/unnamed-chunk-8-1.png)<!-- -->


**3. How do the distributions of life expectancy compare for the years 1952 and 2007?**

```r
gapminder2 %>% 
  filter(year==1952) %>% 
  ggplot(aes(x = life_exp)) +
 geom_histogram(aes(y = after_stat(density)), fill = "skyblue", alpha = 0.4, color = "black", bins=40)+
  geom_density(color = "red")+
  labs(title = "Distribution of Life Expectancy, 1952",
        x="Life Expectancy")
```

![](lab11_hw_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

```r
gapminder2 %>% 
  filter(year==2007) %>% 
  ggplot(aes(x = life_exp)) +
  geom_histogram(aes(y = after_stat(density)), fill = "lemonchiffon", alpha = 0.4, color = "black", bins=40)+
  geom_density(color = "red")+
  labs(title = "Distribution of Life Expectancy, 2007",
        x="Life Expectancy")
```

![](lab11_hw_files/figure-html/unnamed-chunk-10-1.png)<!-- -->


**4. Your answer above doesn't tell the whole story since life expectancy varies by region. Make a summary that shows the min, mean, and max life expectancy by continent for all years represented in the data.**


```r
gapminder2 %>% 
  group_by(continent) %>% 
  select(continent, life_exp) %>%
  summarise(min=min(life_exp),
            max=max(life_exp),
            mean=mean(life_exp))
```

```
## # A tibble: 5 × 4
##   continent   min   max  mean
##   <fct>     <dbl> <dbl> <dbl>
## 1 Africa     23.6  76.4  48.9
## 2 Americas   37.6  80.7  64.7
## 3 Asia       28.8  82.6  60.1
## 4 Europe     43.6  81.8  71.9
## 5 Oceania    69.1  81.2  74.3
```

```r
gapminder2 %>% 
  select(continent, life_exp) %>% 
  group_by(continent) %>% 
   summarise(min=min(life_exp),
            max=max(life_exp),
            mean=mean(life_exp)) %>% 
  ggplot(aes(x = continent, y = mean, color=continent)) +
  geom_point(size = 3) +
  geom_errorbar(aes(ymin = min, ymax = max), width = 0.1) +
  labs(title = "Life Expectancy by Continent",
       x = "Continent",
       y = "Life Expectancy")
```

![](lab11_hw_files/figure-html/unnamed-chunk-12-1.png)<!-- -->


**5. How has life expectancy changed between 1952-2007 for each continent?**

```r
gapminder2 %>% 
  group_by(year, life_exp, continent) %>% 
  summarise(n=n(), .groups='keep') %>% 
  ggplot(aes(x=year, y=life_exp, fill=continent))+
  geom_boxplot(alpha=0.4)+
  theme(axis.text.x = element_text(angle = 60, hjust = 1))+
  labs(title = "Changes in Life Expectancy, 1952 - 2007 ",
       x = "Year",
       fill = "continent")
```

![](lab11_hw_files/figure-html/unnamed-chunk-13-1.png)<!-- -->


**6. We are interested in the relationship between per capita GDP and life expectancy; i.e. does having more money help you live longer?**

```r
gapminder %>% 
  ggplot(aes(x = gdp_percap, y = life_exp, color = continent)) +
  geom_point(alpha=0.4) +
  labs(title = "Per Capita GDP & Life Expectancy, 1952 - 2007 ",
       x = "Per Capita GDP",
       y = "Life Expectancy",
       fill = "Continent")
```

![](lab11_hw_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

This is so overplotted it's hard to read. I'm going to facet it to make it clearer.

```r
gapminder %>% 
  ggplot(aes(x=gdp_percap, y=life_exp, color=continent))+ 
  geom_point(alpha=0.4)+ 
  facet_grid(.~continent)+
  theme(axis.text.x = element_text(angle = 60, hjust = 1))+
  labs(title = "Per Capita GDP & Life Expectancy, 1952 - 2007 ",
       x = "Per Capita GDP",
       y = "Life Expectancy")
```

![](lab11_hw_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

**7. Which countries have had the largest population growth since 1952?**


```r
gapminder %>% 
  filter(year %in% c(1952, 2007)) %>% 
  group_by(country) %>% 
  mutate(growth = last(pop) - first(pop)) %>% 
  ungroup() %>% 
  select(country, growth) %>% 
  arrange(desc(growth)) %>% 
  distinct(country, .keep_all = TRUE) %>% 
  slice_head(n=5)
```

```
## # A tibble: 5 × 2
##   country          growth
##   <fct>             <int>
## 1 China         762419569
## 2 India         738396331
## 3 United States 143586947
## 4 Indonesia     141495000
## 5 Brazil        133408087
```


**8. Use your results from the question above to plot population growth for the top five countries since 1952.**

```r
gapminder %>% 
  filter(year %in% c(1952, 2007)) %>% 
  group_by(country) %>% 
  mutate(growth = last(pop) - first(pop)) %>% 
  ungroup() %>% 
  select(country, growth) %>% 
  arrange(desc(growth)) %>% 
  distinct(country, .keep_all = TRUE) %>% 
  slice_head(n=5) %>% 
  ggplot(aes(x = reorder(country,-growth), fill=country, y = growth))+
  geom_col(alpha=0.5)+
  labs(title="Population Growth 1952 - 2007 in the Top 5 Countries",
       x=NULL,
       y="Growth")
```

![](lab11_hw_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

**9. How does per-capita GDP growth compare between these same five countries?**


```r
gapminder %>% 
  filter(country =="China"|country =="India"|country=="Brazil"|country=="Indonesia"|
         country=="United States") %>%
  filter(year %in% c(1952, 2007)) %>% 
  group_by(country) %>% 
  mutate(gdp_growth = last(gdp_percap) - first(gdp_percap)) %>% 
  ungroup() %>% 
  select(country, gdp_growth) %>% 
  arrange(-gdp_growth) %>% 
  distinct(country, .keep_all = TRUE)
```

```
## # A tibble: 5 × 2
##   country       gdp_growth
##   <fct>              <dbl>
## 1 United States     28961.
## 2 Brazil             6957.
## 3 China              4559.
## 4 Indonesia          2791.
## 5 India              1906.
```

```r
gapminder %>% 
  filter(country =="China"|country =="India"|country=="Brazil"|country=="Indonesia"|
         country=="United States") %>%
  filter(year %in% c(1952, 2007)) %>% 
  group_by(country) %>% 
  mutate(gdp_growth = last(gdp_percap) - first(gdp_percap)) %>% 
  ungroup() %>% 
  select(country, gdp_growth) %>% 
  arrange(-gdp_growth) %>% 
  distinct(country, .keep_all = TRUE) %>% 
  ggplot(aes(x = reorder(country,-gdp_growth), fill=country, y = gdp_growth))+
  geom_col(alpha=0.5)+
  labs(title="Per Capita GDP Growth 1952 - 2007 in the Top 5 Countries",
       x=NULL,
       y="Per Capita GDP Growth")
```

![](lab11_hw_files/figure-html/unnamed-chunk-19-1.png)<!-- -->


**10. Make one plot of your choice that uses faceting!**


```r
gapminder %>% 
  filter(country =="China"|country =="India"|country=="Brazil"|country=="Indonesia"|
         country=="United States") %>%
  ggplot(aes(x=gdp_percap, y=life_exp, color=country))+ 
  geom_point(alpha=0.4)+ 
  facet_grid(.~country)+
  theme(axis.text.x = element_text(angle = 60, hjust = 1))+
  labs(title = "Per Capita GDP & Life Expectancy, 1952 - 2007 ",
       x = "Per Capita GDP",
       y = "Life Expectancy")
```

![](lab11_hw_files/figure-html/unnamed-chunk-20-1.png)<!-- -->


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
