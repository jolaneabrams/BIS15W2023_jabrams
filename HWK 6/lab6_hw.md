---
title: "Lab 6 Homework"
author: "Jolane Abrams"
date: "2023-01-29"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(skimr)
```

For this assignment we are going to work with a large data set from the [United Nations Food and Agriculture Organization](http://www.fao.org/about/en/) on world fisheries. These data are pretty wild, so we need to do some cleaning. First, load the data.  

Load the data `FAO_1950to2012_111914.csv` as a new object titled `fisheries`.

```r
fisheries <- readr::read_csv(file = "data/FAO_1950to2012_111914.csv")
```

```
## Rows: 17692 Columns: 71
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (69): Country, Common name, ISSCAAP taxonomic group, ASFIS species#, ASF...
## dbl  (2): ISSCAAP group#, FAO major fishing area
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

1. Do an exploratory analysis of the data (your choice). What are the names of the variables, what are the dimensions, are there any NA's, what are the classes of the variables?  

```r
names(fisheries)
```

```
##  [1] "Country"                 "Common name"            
##  [3] "ISSCAAP group#"          "ISSCAAP taxonomic group"
##  [5] "ASFIS species#"          "ASFIS species name"     
##  [7] "FAO major fishing area"  "Measure"                
##  [9] "1950"                    "1951"                   
## [11] "1952"                    "1953"                   
## [13] "1954"                    "1955"                   
## [15] "1956"                    "1957"                   
## [17] "1958"                    "1959"                   
## [19] "1960"                    "1961"                   
## [21] "1962"                    "1963"                   
## [23] "1964"                    "1965"                   
## [25] "1966"                    "1967"                   
## [27] "1968"                    "1969"                   
## [29] "1970"                    "1971"                   
## [31] "1972"                    "1973"                   
## [33] "1974"                    "1975"                   
## [35] "1976"                    "1977"                   
## [37] "1978"                    "1979"                   
## [39] "1980"                    "1981"                   
## [41] "1982"                    "1983"                   
## [43] "1984"                    "1985"                   
## [45] "1986"                    "1987"                   
## [47] "1988"                    "1989"                   
## [49] "1990"                    "1991"                   
## [51] "1992"                    "1993"                   
## [53] "1994"                    "1995"                   
## [55] "1996"                    "1997"                   
## [57] "1998"                    "1999"                   
## [59] "2000"                    "2001"                   
## [61] "2002"                    "2003"                   
## [63] "2004"                    "2005"                   
## [65] "2006"                    "2007"                   
## [67] "2008"                    "2009"                   
## [69] "2010"                    "2011"                   
## [71] "2012"
```

```r
dim(fisheries)
```

```
## [1] 17692    71
```

```r
anyNA(fisheries)
```

```
## [1] TRUE
```

```r
class(fisheries)
```

```
## [1] "spec_tbl_df" "tbl_df"      "tbl"         "data.frame"
```

2. Use `janitor` to rename the columns and make them easier to use. As part of this cleaning step, change `country`, `isscaap_group_number`, `asfis_species_number`, and `fao_major_fishing_area` to data class factor. 

```r
fisheries <- janitor::clean_names(fisheries)
names(fisheries)
```

```
##  [1] "country"                 "common_name"            
##  [3] "isscaap_group_number"    "isscaap_taxonomic_group"
##  [5] "asfis_species_number"    "asfis_species_name"     
##  [7] "fao_major_fishing_area"  "measure"                
##  [9] "x1950"                   "x1951"                  
## [11] "x1952"                   "x1953"                  
## [13] "x1954"                   "x1955"                  
## [15] "x1956"                   "x1957"                  
## [17] "x1958"                   "x1959"                  
## [19] "x1960"                   "x1961"                  
## [21] "x1962"                   "x1963"                  
## [23] "x1964"                   "x1965"                  
## [25] "x1966"                   "x1967"                  
## [27] "x1968"                   "x1969"                  
## [29] "x1970"                   "x1971"                  
## [31] "x1972"                   "x1973"                  
## [33] "x1974"                   "x1975"                  
## [35] "x1976"                   "x1977"                  
## [37] "x1978"                   "x1979"                  
## [39] "x1980"                   "x1981"                  
## [41] "x1982"                   "x1983"                  
## [43] "x1984"                   "x1985"                  
## [45] "x1986"                   "x1987"                  
## [47] "x1988"                   "x1989"                  
## [49] "x1990"                   "x1991"                  
## [51] "x1992"                   "x1993"                  
## [53] "x1994"                   "x1995"                  
## [55] "x1996"                   "x1997"                  
## [57] "x1998"                   "x1999"                  
## [59] "x2000"                   "x2001"                  
## [61] "x2002"                   "x2003"                  
## [63] "x2004"                   "x2005"                  
## [65] "x2006"                   "x2007"                  
## [67] "x2008"                   "x2009"                  
## [69] "x2010"                   "x2011"                  
## [71] "x2012"
```


```r
fisheries <- 
  fisheries %>% 
  mutate(across(c(country, isscaap_group_number, asfis_species_number, fao_major_fishing_area), as_factor))
```


```r
glimpse(fisheries)
```

```
## Rows: 17,692
## Columns: 71
```

```
## Warning in grepl(",", levels(x), fixed = TRUE): input string 43 is invalid UTF-8
```

```
## Warning in grepl(",", levels(x), fixed = TRUE): input string 45 is invalid UTF-8
```

```
## Warning in grepl(",", levels(x), fixed = TRUE): input string 149 is invalid
## UTF-8
```

```
## Warning in grepl(",", levels(x), fixed = TRUE): input string 150 is invalid
## UTF-8
```

```
## $ country                 <fct> "Albania", "Albania", "Albania", "Albania", "A…
## $ common_name             <chr> "Angelsharks, sand devils nei", "Atlantic boni…
## $ isscaap_group_number    <fct> 38, 36, 37, 45, 32, 37, 33, 45, 38, 57, 33, 57…
## $ isscaap_taxonomic_group <chr> "Sharks, rays, chimaeras", "Tunas, bonitos, bi…
## $ asfis_species_number    <fct> 10903XXXXX, 1750100101, 17710001XX, 2280203101…
## $ asfis_species_name      <chr> "Squatinidae", "Sarda sarda", "Sphyraena spp",…
## $ fao_major_fishing_area  <fct> 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, 37, 37…
## $ measure                 <chr> "Quantity (tonnes)", "Quantity (tonnes)", "Qua…
## $ x1950                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1951                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1952                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1953                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1954                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1955                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1956                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1957                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1958                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1959                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1960                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1961                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1962                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1963                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1964                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1965                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1966                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1967                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1968                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1969                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1970                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1971                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1972                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1973                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1974                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1975                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1976                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1977                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1978                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1979                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1980                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1981                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1982                   <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…
## $ x1983                   <chr> NA, NA, NA, NA, NA, NA, "559", NA, NA, NA, NA,…
## $ x1984                   <chr> NA, NA, NA, NA, NA, NA, "392", NA, NA, NA, NA,…
## $ x1985                   <chr> NA, NA, NA, NA, NA, NA, "406", NA, NA, NA, NA,…
## $ x1986                   <chr> NA, NA, NA, NA, NA, NA, "499", NA, NA, NA, NA,…
## $ x1987                   <chr> NA, NA, NA, NA, NA, NA, "564", NA, NA, NA, NA,…
## $ x1988                   <chr> NA, NA, NA, NA, NA, NA, "724", NA, NA, NA, NA,…
## $ x1989                   <chr> NA, NA, NA, NA, NA, NA, "583", NA, NA, NA, NA,…
## $ x1990                   <chr> NA, NA, NA, NA, NA, NA, "754", NA, NA, NA, NA,…
## $ x1991                   <chr> NA, NA, NA, NA, NA, NA, "283", NA, NA, NA, NA,…
## $ x1992                   <chr> NA, NA, NA, NA, NA, NA, "196", NA, NA, NA, NA,…
## $ x1993                   <chr> NA, NA, NA, NA, NA, NA, "150 F", NA, NA, NA, N…
## $ x1994                   <chr> NA, NA, NA, NA, NA, NA, "100 F", NA, NA, NA, N…
## $ x1995                   <chr> "0 0", "1", NA, "0 0", "0 0", NA, "52", "30", …
## $ x1996                   <chr> "53", "2", NA, "3", "2", NA, "104", "8", NA, "…
## $ x1997                   <chr> "20", "0 0", NA, "0 0", "0 0", NA, "65", "4", …
## $ x1998                   <chr> "31", "12", NA, NA, NA, NA, "220", "18", NA, "…
## $ x1999                   <chr> "30", "30", NA, NA, NA, NA, "220", "18", NA, "…
## $ x2000                   <chr> "30", "25", "2", NA, NA, NA, "220", "20", NA, …
## $ x2001                   <chr> "16", "30", NA, NA, NA, NA, "120", "23", NA, "…
## $ x2002                   <chr> "79", "24", NA, "34", "6", NA, "150", "84", NA…
## $ x2003                   <chr> "1", "4", NA, "22", NA, NA, "84", "178", NA, "…
## $ x2004                   <chr> "4", "2", "2", "15", "1", "2", "76", "285", "1…
## $ x2005                   <chr> "68", "23", "4", "12", "5", "6", "68", "150", …
## $ x2006                   <chr> "55", "30", "7", "18", "8", "9", "86", "102", …
## $ x2007                   <chr> "12", "19", NA, NA, NA, NA, "132", "18", NA, "…
## $ x2008                   <chr> "23", "27", NA, NA, NA, NA, "132", "23", NA, "…
## $ x2009                   <chr> "14", "21", NA, NA, NA, NA, "154", "20", NA, "…
## $ x2010                   <chr> "78", "23", "7", NA, NA, NA, "80", "228", NA, …
## $ x2011                   <chr> "12", "12", NA, NA, NA, NA, "88", "9", NA, "90…
## $ x2012                   <chr> "5", "5", NA, NA, NA, NA, "129", "290", NA, "1…
```

We need to deal with the years because they are being treated as characters and start with an X. We also have the problem that the column names that are years actually represent data. We haven't discussed tidy data yet, so here is some help. You should run this ugly chunk to transform the data for the rest of the homework. It will only work if you have used janitor to rename the variables in question 2!

```r
fisheries_tidy <- fisheries %>% 
pivot_longer(-c(country,common_name,isscaap_group_number,isscaap_taxonomic_group,asfis_species_number,asfis_species_name,fao_major_fishing_area,measure),
         names_to = "year",
         values_to = "catch",
         values_drop_na = TRUE) %>% 
mutate(year= as.numeric(str_replace(year, 'x', ''))) %>% 
mutate(catch= str_replace(catch, c(' F'), replacement = '')) %>% 
mutate(catch= str_replace(catch, c('...'), replacement = '')) %>% 
mutate(catch= str_replace(catch, c('-'), replacement = '')) %>% 
mutate(catch= str_replace(catch, c('0 0'), replacement = ''))

fisheries_tidy$catch <- as.numeric(fisheries_tidy$catch)
```

3. How many countries are represented in the data? Provide a count and list their names.

204 countries (via summarise below); names via tabyl below

```r
fisheries %>% 
  summarise(n_country=n_distinct(country))
```

```
## # A tibble: 1 × 1
##   n_country
##       <int>
## 1       204
```

```r
fisheries %>% 
tabyl(country)  
```

```
##                    country   n      percent
##                    Albania  67 3.787022e-03
##                    Algeria  59 3.334841e-03
##             American Samoa  32 1.808727e-03
##                     Angola  88 4.974000e-03
##                   Anguilla   3 1.695682e-04
##        Antigua and Barbuda  22 1.243500e-03
##                  Argentina 140 7.913181e-03
##                      Aruba   5 2.826136e-04
##                  Australia 301 1.701334e-02
##                    Bahamas  14 7.913181e-04
##                    Bahrain  62 3.504409e-03
##                 Bangladesh   8 4.521818e-04
##                   Barbados  21 1.186977e-03
##                    Belgium  57 3.221795e-03
##                     Belize 133 7.517522e-03
##                      Benin  68 3.843545e-03
##                    Bermuda  47 2.656568e-03
##   Bonaire/S.Eustatius/Saba   2 1.130454e-04
##     Bosnia and Herzegovina   1 5.652272e-05
##                     Brazil 136 7.687090e-03
##   British Indian Ocean Ter   7 3.956591e-04
##     British Virgin Islands  18 1.017409e-03
##          Brunei Darussalam   8 4.521818e-04
##                   Bulgaria 197 1.113498e-02
##                 Cabo Verde  26 1.469591e-03
##                   Cambodia   8 4.521818e-04
##                   Cameroon  36 2.034818e-03
##                     Canada 125 7.065340e-03
##             Cayman Islands   4 2.260909e-04
##            Channel Islands  65 3.673977e-03
##                      Chile 141 7.969704e-03
##                      China 236 1.333936e-02
##       China, Hong Kong SAR  32 1.808727e-03
##           China, Macao SAR   4 2.260909e-04
##                   Colombia  81 4.578340e-03
##                    Comoros  43 2.430477e-03
##    Congo, Dem. Rep. of the  21 1.186977e-03
##         Congo, Republic of  53 2.995704e-03
##               Cook Islands  55 3.108750e-03
##                 Costa Rica  45 2.543522e-03
##                    Croatia 103 5.821840e-03
##                       Cuba 162 9.156681e-03
##                 Cura\xe7ao   9 5.087045e-04
##                     Cyprus  96 5.426181e-03
##           C\xf4te d'Ivoire  67 3.787022e-03
##                    Denmark 103 5.821840e-03
##                   Djibouti  14 7.913181e-04
##                   Dominica  13 7.347954e-04
##         Dominican Republic  50 2.826136e-03
##                    Ecuador 100 5.652272e-03
##                      Egypt  87 4.917477e-03
##                El Salvador  25 1.413068e-03
##          Equatorial Guinea  15 8.478408e-04
##                    Eritrea  49 2.769613e-03
##                    Estonia 184 1.040018e-02
##                   Ethiopia   5 2.826136e-04
##     Falkland Is.(Malvinas)  33 1.865250e-03
##              Faroe Islands  96 5.426181e-03
##          Fiji, Republic of  50 2.826136e-03
##                    Finland  16 9.043636e-04
##                     France 489 2.763961e-02
##              French Guiana  18 1.017409e-03
##           French Polynesia  31 1.752204e-03
##       French Southern Terr   3 1.695682e-04
##                      Gabon  45 2.543522e-03
##                     Gambia  49 2.769613e-03
##                    Georgia  86 4.860954e-03
##                    Germany 306 1.729595e-02
##                      Ghana  94 5.313136e-03
##                  Gibraltar   1 5.652272e-05
##                     Greece 125 7.065340e-03
##                  Greenland  60 3.391363e-03
##                    Grenada  47 2.656568e-03
##                 Guadeloupe   8 4.521818e-04
##                       Guam  23 1.300023e-03
##                  Guatemala  38 2.147863e-03
##                     Guinea  56 3.165272e-03
##               GuineaBissau  32 1.808727e-03
##                     Guyana  11 6.217499e-04
##                      Haiti   6 3.391363e-04
##                   Honduras  68 3.843545e-03
##                    Iceland 107 6.047931e-03
##                      India 108 6.104454e-03
##                  Indonesia 223 1.260457e-02
##     Iran (Islamic Rep. of)  64 3.617454e-03
##                       Iraq  16 9.043636e-04
##                    Ireland 191 1.079584e-02
##                Isle of Man  41 2.317432e-03
##                     Israel  79 4.465295e-03
##                      Italy 261 1.475243e-02
##                    Jamaica   6 3.391363e-04
##                      Japan 611 3.453538e-02
##                     Jordan  12 6.782727e-04
##                      Kenya  31 1.752204e-03
##                   Kiribati  27 1.526113e-03
##   Korea, Dem. People's Rep  14 7.913181e-04
##         Korea, Republic of 620 3.504409e-02
##                     Kuwait  23 1.300023e-03
##                     Latvia 162 9.156681e-03
##                    Lebanon  20 1.130454e-03
##                    Liberia  76 4.295727e-03
##                      Libya  56 3.165272e-03
##                  Lithuania 215 1.215239e-02
##                 Madagascar  35 1.978295e-03
##                   Malaysia 166 9.382772e-03
##                   Maldives  12 6.782727e-04
##                      Malta 104 5.878363e-03
##           Marshall Islands  13 7.347954e-04
##                 Martinique  16 9.043636e-04
##                 Mauritania  93 5.256613e-03
##                  Mauritius  30 1.695682e-03
##                    Mayotte  14 7.913181e-04
##                     Mexico 198 1.119150e-02
##  Micronesia, Fed.States of  13 7.347954e-04
##                     Monaco   1 5.652272e-05
##                 Montenegro  24 1.356545e-03
##                 Montserrat   1 5.652272e-05
##                    Morocco 153 8.647976e-03
##                 Mozambique  30 1.695682e-03
##                    Myanmar   5 2.826136e-04
##                    Namibia  76 4.295727e-03
##                      Nauru   9 5.087045e-04
##                Netherlands 155 8.761022e-03
##       Netherlands Antilles  17 9.608863e-04
##              New Caledonia  26 1.469591e-03
##                New Zealand 208 1.175673e-02
##                  Nicaragua  59 3.334841e-03
##                    Nigeria  50 2.826136e-03
##                       Niue  11 6.217499e-04
##             Norfolk Island   1 5.652272e-05
##       Northern Mariana Is.  19 1.073932e-03
##                     Norway 188 1.062627e-02
##                       Oman  53 2.995704e-03
##                  Other nei 100 5.652272e-03
##                   Pakistan  60 3.391363e-03
##                      Palau  33 1.865250e-03
##    Palestine, Occupied Tr.  25 1.413068e-03
##                     Panama 121 6.839249e-03
##           Papua New Guinea  20 1.130454e-03
##                       Peru  54 3.052227e-03
##                Philippines 138 7.800136e-03
##           Pitcairn Islands   1 5.652272e-05
##                     Poland 263 1.486548e-02
##                   Portugal 763 4.312684e-02
##                Puerto Rico  49 2.769613e-03
##                      Qatar  35 1.978295e-03
##                    Romania 191 1.079584e-02
##         Russian Federation 523 2.956138e-02
##                 R\xe9union  37 2.091341e-03
##        Saint Barth\xe9lemy   1 5.652272e-05
##               Saint Helena  19 1.073932e-03
##      Saint Kitts and Nevis  28 1.582636e-03
##                Saint Lucia  27 1.526113e-03
##   Saint Vincent/Grenadines  71 4.013113e-03
##                SaintMartin   1 5.652272e-05
##                      Samoa  15 8.478408e-04
##      Sao Tome and Principe  35 1.978295e-03
##               Saudi Arabia 128 7.234908e-03
##                    Senegal 140 7.913181e-03
##      Serbia and Montenegro  45 2.543522e-03
##                 Seychelles  70 3.956591e-03
##               Sierra Leone  59 3.334841e-03
##                  Singapore  40 2.260909e-03
##               Sint Maarten   2 1.130454e-04
##                   Slovenia  50 2.826136e-03
##            Solomon Islands  18 1.017409e-03
##                    Somalia   3 1.695682e-04
##               South Africa 200 1.130454e-02
##                      Spain 915 5.171829e-02
##                  Sri Lanka  34 1.921773e-03
##    St. Pierre and Miquelon  40 2.260909e-03
##                      Sudan   3 1.695682e-04
##             Sudan (former)   3 1.695682e-04
##                   Suriname  27 1.526113e-03
##     Svalbard and Jan Mayen   1 5.652272e-05
##                     Sweden  72 4.069636e-03
##       Syrian Arab Republic  34 1.921773e-03
##   Taiwan Province of China 310 1.752204e-02
##   Tanzania, United Rep. of  49 2.769613e-03
##                   Thailand 150 8.478408e-03
##                 TimorLeste   7 3.956591e-04
##                       Togo  78 4.408772e-03
##                    Tokelau  10 5.652272e-04
##                      Tonga  14 7.913181e-04
##        Trinidad and Tobago  39 2.204386e-03
##                    Tunisia  85 4.804431e-03
##                     Turkey  76 4.295727e-03
##       Turks and Caicos Is.   6 3.391363e-04
##                     Tuvalu  10 5.652272e-04
##          US Virgin Islands  29 1.639159e-03
##                    Ukraine 294 1.661768e-02
##         Un. Sov. Soc. Rep. 515 2.910920e-02
##       United Arab Emirates  52 2.939182e-03
##             United Kingdom 362 2.046123e-02
##   United States of America 627 3.543975e-02
##                    Uruguay 131 7.404477e-03
##                    Vanuatu  71 4.013113e-03
##    Venezuela, Boliv Rep of  80 4.521818e-03
##                   Viet Nam  14 7.913181e-04
##      Wallis and Futuna Is.   5 2.826136e-04
##             Western Sahara   3 1.695682e-04
##                      Yemen  39 2.204386e-03
##             Yugoslavia SFR  36 2.034818e-03
##                   Zanzibar  19 1.073932e-03
```

4. Refocus the data only to include country, isscaap_taxonomic_group, asfis_species_name, asfis_species_number, year, catch.

```r
select(fisheries_tidy,"country", "isscaap_taxonomic_group", "asfis_species_name", "asfis_species_number", "year", "catch")
```

```
## # A tibble: 376,771 × 6
##    country isscaap_taxonomic_group asfis_species_name asfis_specie…¹  year catch
##    <fct>   <chr>                   <chr>              <fct>          <dbl> <dbl>
##  1 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      1995    NA
##  2 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      1996    53
##  3 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      1997    20
##  4 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      1998    31
##  5 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      1999    30
##  6 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      2000    30
##  7 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      2001    16
##  8 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      2002    79
##  9 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      2003     1
## 10 Albania Sharks, rays, chimaeras Squatinidae        10903XXXXX      2004     4
## # … with 376,761 more rows, and abbreviated variable name ¹​asfis_species_number
```

5. Based on the asfis_species_number, how many distinct fish species were caught as part of these data?

```r
fisheries %>% 
  summarise(n_species=n_distinct(asfis_species_number))
```

```
## # A tibble: 1 × 1
##   n_species
##       <int>
## 1      1553
```

6. Which country had the largest overall catch in the year 2000?


```r
fisheries_tidy %>% 
  filter(year == 2000) %>% 
  select("country", "catch", "year") %>% 
  arrange(-catch)
```

```
## # A tibble: 8,793 × 3
##    country                  catch  year
##    <fct>                    <dbl> <dbl>
##  1 China                     9068  2000
##  2 Peru                      5717  2000
##  3 Russian Federation        5065  2000
##  4 Viet Nam                  4945  2000
##  5 Chile                     4299  2000
##  6 China                     3288  2000
##  7 China                     2782  2000
##  8 United States of America  2438  2000
##  9 China                     1234  2000
## 10 Philippines                999  2000
## # … with 8,783 more rows
```
China had the biggest overall catch in 2000.

7. Which country caught the most sardines (_Sardina pilchardus_) between the years 1990-2000?

```r
fisheries_tidy %>% 
  filter(between(year,1990, 2000) & asfis_species_name =="Sardina pilchardus") %>% 
  group_by(country) %>% 
  select(country, year, catch) %>% 
  mutate(total_catch=sum(catch)) %>%
  arrange(desc(total_catch)) 
```

```
## # A tibble: 336 × 4
## # Groups:   country [37]
##    country  year catch total_catch
##    <fct>   <dbl> <dbl>       <dbl>
##  1 Morocco  1990   845        7470
##  2 Morocco  1991   827        7470
##  3 Morocco  1992   352        7470
##  4 Morocco  1993   710        7470
##  5 Morocco  1994   947        7470
##  6 Morocco  1995   152        7470
##  7 Morocco  1996   925        7470
##  8 Morocco  1997   496        7470
##  9 Morocco  1998   723        7470
## 10 Morocco  1999   171        7470
## # … with 326 more rows
```
Morocco caught the most sardines between 1990-2000.


8. Which five countries caught the most cephalopods between 2008-2012?

```r
fisheries_tidy %>% 
  filter(between(year,2008, 2012) & common_name =="Cephalopods nei") %>%
  group_by(country) %>% 
  select(country, year, catch) %>% 
  mutate(total_catch=sum(catch)) %>%
  arrange(desc(total_catch)) 
```

```
## # A tibble: 80 × 4
## # Groups:   country [16]
##    country  year catch total_catch
##    <fct>   <dbl> <dbl>       <dbl>
##  1 India    2008    13         570
##  2 India    2009    61         570
##  3 India    2010    93         570
##  4 India    2011    35         570
##  5 India    2012    88         570
##  6 India    2008     8         570
##  7 India    2009    62         570
##  8 India    2010    60         570
##  9 India    2011    94         570
## 10 India    2012    56         570
## # … with 70 more rows
```
India caught the most cephalopods between 2008-2012, with 570 metric tonnes, followed by China with 257, Algeria with 162, France with 101, and TimorLeste with 76.


9. Which species had the highest catch total between 2008-2012? (hint: Osteichthyes is not a species)


```r
fisheries_tidy %>% 
  filter(between(year,2008, 2012)) %>%
  group_by(asfis_species_name) %>% 
  select(asfis_species_name, year, catch) %>% 
  mutate(total_catch=sum(catch)) %>%
  arrange(desc(total_catch)) 
```

```
## # A tibble: 51,014 × 4
## # Groups:   asfis_species_name [1,472]
##    asfis_species_name     year catch total_catch
##    <chr>                 <dbl> <dbl>       <dbl>
##  1 Theragra chalcogramma  2008     2       41075
##  2 Theragra chalcogramma  2009     2       41075
##  3 Theragra chalcogramma  2010     6       41075
##  4 Theragra chalcogramma  2011     9       41075
##  5 Theragra chalcogramma  2012     1       41075
##  6 Theragra chalcogramma  2008    38       41075
##  7 Theragra chalcogramma  2009   261       41075
##  8 Theragra chalcogramma  2010   166       41075
##  9 Theragra chalcogramma  2011   920       41075
## 10 Theragra chalcogramma  2012   823       41075
## # … with 51,004 more rows
```
Theragra chalcogramma had the highest total catch between 2008-2012: 41075 metric tonnes. 

10. Use the data to do at least one analysis of your choice.

Which countries caught the most shrimp and lobsters between 1970-2000?


```r
fisheries_tidy %>% 
  filter(between(year,1970, 2000) & (isscaap_taxonomic_group  =="Shrimps, prawns"|isscaap_taxonomic_group =="Lobsters, spinyrock lobsters")) %>%
    group_by(country) %>% 
    select(country, year, catch) %>% 
    mutate(total_catch=sum(catch)) %>%
    arrange(desc(total_catch)) 
```

```
## # A tibble: 12,487 × 4
## # Groups:   country [157]
##    country  year catch total_catch
##    <fct>   <dbl> <dbl>       <dbl>
##  1 India    1988    91       18087
##  2 India    1989    13       18087
##  3 India    1990    73       18087
##  4 India    1991    10       18087
##  5 India    1992    67       18087
##  6 India    1993    11       18087
##  7 India    1994    59       18087
##  8 India    1995    61       18087
##  9 India    1996    51       18087
## 10 India    1997    89       18087
## # … with 12,477 more rows
```
India was far and away the leader, with 18087 metric tonnes, followed at a distance by Iceland (1078 tonnes), Ukraine (151 tonnes), and St. Kitts (136 tonnes). 



## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   

  ```
