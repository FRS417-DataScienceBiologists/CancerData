---
title: "Cancer Presentation"
author: "Lara and Jonathan"
date: "Winter Quarter 2019"
output: 
  ioslides_presentation: 
    keep_md: yes
---

##Background
- Over 1.5 million cancer cases diagnosed in the US each year
- Darthmouth Atlas Project report
- Examining elderly cancer patients across the country
- Poor prognosis patients

<img src="PPT_IMG_1.jpg" width="80%" style="display: block; margin: auto;" />



## Load the tidyverse 

```r
library(tidyverse)
```

```
## -- Attaching packages ----------------------------------------------------------------------------------------------------- tidyverse 1.2.1 --
```

```
## v ggplot2 3.1.0     v purrr   0.2.5
## v tibble  1.4.2     v dplyr   0.7.8
## v tidyr   0.8.2     v stringr 1.3.1
## v readr   1.3.1     v forcats 0.3.0
```

```
## -- Conflicts -------------------------------------------------------------------------------------------------------- tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

## Retrieve data from Excel

```r
Cancer <-
readr::read_csv("DAP_cancer_events_hosp_2010.csv", 
                na = c("", " ", "NA", "#N/A", "-999", "\\"))
```

```
## Parsed with column specification:
## cols(
##   `Provider ID` = col_double(),
##   `Hospital Name` = col_character(),
##   City = col_character(),
##   State = col_character(),
##   `Number of deaths among cancer patients assigned to hospital (2010)` = col_number(),
##   `Percent of cancer patients dying in hospital (2010)` = col_double(),
##   `Percent of cancer patients admitted to hospital during the last month of life (2010 deaths)` = col_double(),
##   `Hospital days per cancer patient during the last month of life (2010 deaths)` = col_double(),
##   `Percent of cancer patients admitted to intensive care during the last month of life (2010 deaths)` = col_double(),
##   `ICU days per cancer patient during the last month of life (2010 deaths)` = col_double(),
##   `Percent of cancer patients receiving life-sustaining treatment during the last month of life (2010 deaths)` = col_double(),
##   `Percent of cancer patients receiving chemotherapy during the last two weeks of life (2010 deaths)` = col_double(),
##   `Percent of cancer patients enrolled in hospice during the last month of life (2010 deaths)` = col_double(),
##   `Hospice days per cancer patient during the last month of life (2010 deaths)` = col_double(),
##   `Percent of cancer patients enrolled in hospice during the last three days of life (2010 deaths)` = col_double(),
##   `Percent of cancer patients seeing ten or more physicians during the last six months of life (2010 deaths)` = col_double(),
##   `HIV Related` = col_character()
## )
```

```r
Cancer
```

```
## # A tibble: 953 x 17
##    `Provider ID` `Hospital Name` City  State `Number of deat~
##            <dbl> <chr>           <chr> <chr>            <dbl>
##  1         10006 Eliza Coffee M~ Flor~ AL                 119
##  2         10024 Jackson Hospit~ Mont~ AL                  93
##  3         10029 East Alabama M~ Opel~ AL                  86
##  4         10033 University of ~ Birm~ AL                 211
##  5         10039 Huntsville Hos~ Hunt~ AL                 331
##  6         10055 Flowers Hospit~ Doth~ AL                  81
##  7         10056 St. Vincent's ~ Birm~ AL                 102
##  8         10078 Northeast Alab~ Anni~ AL                  88
##  9         10085 Decatur Genera~ Deca~ AL                  81
## 10         10090 Providence Hos~ Mobi~ AL                  94
## # ... with 943 more rows, and 12 more variables: `Percent of cancer
## #   patients dying in hospital (2010)` <dbl>, `Percent of cancer patients
## #   admitted to hospital during the last month of life (2010
## #   deaths)` <dbl>, `Hospital days per cancer patient during the last
## #   month of life (2010 deaths)` <dbl>, `Percent of cancer patients
## #   admitted to intensive care during the last month of life (2010
## #   deaths)` <dbl>, `ICU days per cancer patient during the last month of
## #   life (2010 deaths)` <dbl>, `Percent of cancer patients receiving
## #   life-sustaining treatment during the last month of life (2010
## #   deaths)` <dbl>, `Percent of cancer patients receiving chemotherapy
## #   during the last two weeks of life (2010 deaths)` <dbl>, `Percent of
## #   cancer patients enrolled in hospice during the last month of life
## #   (2010 deaths)` <dbl>, `Hospice days per cancer patient during the last
## #   month of life (2010 deaths)` <dbl>, `Percent of cancer patients
## #   enrolled in hospice during the last three days of life (2010
## #   deaths)` <dbl>, `Percent of cancer patients seeing ten or more
## #   physicians during the last six months of life (2010 deaths)` <dbl>,
## #   `HIV Related` <chr>
```

## Briefly analyze data and look for NAs

```r
glimpse(Cancer)
```

```
## Observations: 953
## Variables: 17
## $ `Provider ID`                                                                                                <dbl> ...
## $ `Hospital Name`                                                                                              <chr> ...
## $ City                                                                                                         <chr> ...
## $ State                                                                                                        <chr> ...
## $ `Number of deaths among cancer patients assigned to hospital (2010)`                                         <dbl> ...
## $ `Percent of cancer patients dying in hospital (2010)`                                                        <dbl> ...
## $ `Percent of cancer patients admitted to hospital during the last month of life (2010 deaths)`                <dbl> ...
## $ `Hospital days per cancer patient during the last month of life (2010 deaths)`                               <dbl> ...
## $ `Percent of cancer patients admitted to intensive care during the last month of life (2010 deaths)`          <dbl> ...
## $ `ICU days per cancer patient during the last month of life (2010 deaths)`                                    <dbl> ...
## $ `Percent of cancer patients receiving life-sustaining treatment during the last month of life (2010 deaths)` <dbl> ...
## $ `Percent of cancer patients receiving chemotherapy during the last two weeks of life (2010 deaths)`          <dbl> ...
## $ `Percent of cancer patients enrolled in hospice during the last month of life (2010 deaths)`                 <dbl> ...
## $ `Hospice days per cancer patient during the last month of life (2010 deaths)`                                <dbl> ...
## $ `Percent of cancer patients enrolled in hospice during the last three days of life (2010 deaths)`            <dbl> ...
## $ `Percent of cancer patients seeing ten or more physicians during the last six months of life (2010 deaths)`  <dbl> ...
## $ `HIV Related`                                                                                                <chr> ...
```

## Load skimr

```r
library(skimr)
```

```
## Warning: package 'skimr' was built under R version 3.5.3
```

```
## 
## Attaching package: 'skimr'
```

```
## The following object is masked from 'package:stats':
## 
##     filter
```

## Use the Skim function

```r
Cancer %>%
  skimr::skim()
```

```
## Skim summary statistics
##  n obs: 953 
##  n variables: 17 
## 
## -- Variable type:character -------------------------------------------------------------------------------------------------------------------
##       variable missing complete   n min max empty n_unique
##           City       2      951 953   4  20     0      570
##    HIV Related       0      953 953   3   5     0        2
##  Hospital Name       0      953 953  10  34     0      815
##          State       2      951 953   2   2     0       50
## 
## -- Variable type:numeric ---------------------------------------------------------------------------------------------------------------------
##                                                                                                    variable
##                                 Hospice days per cancer patient during the last month of life (2010 deaths)
##                                Hospital days per cancer patient during the last month of life (2010 deaths)
##                                     ICU days per cancer patient during the last month of life (2010 deaths)
##                                          Number of deaths among cancer patients assigned to hospital (2010)
##                 Percent of cancer patients admitted to hospital during the last month of life (2010 deaths)
##           Percent of cancer patients admitted to intensive care during the last month of life (2010 deaths)
##                                                         Percent of cancer patients dying in hospital (2010)
##                  Percent of cancer patients enrolled in hospice during the last month of life (2010 deaths)
##             Percent of cancer patients enrolled in hospice during the last three days of life (2010 deaths)
##           Percent of cancer patients receiving chemotherapy during the last two weeks of life (2010 deaths)
##  Percent of cancer patients receiving life-sustaining treatment during the last month of life (2010 deaths)
##   Percent of cancer patients seeing ten or more physicians during the last six months of life (2010 deaths)
##                                                                                                 Provider ID
##  missing complete   n      mean        sd      p0       p25      p50
##        0      953 953      8.62      1.9      2.2      7.4       8.6
##        0      953 953      5.43      0.96     2.3      4.8       5.4
##       15      938 953      1.59      0.96     0.1      0.9       1.3
##        0      953 953    554.83   8832.81    80       99       124  
##        0      953 953     65.23      5.89    47.1     61.8      65.4
##       15      938 953     31.11     11.75     7.9     22.13     30.3
##       12      941 953     26.52      8.14     7       20.6      26.1
##        0      953 953     61.42     11.58    16.4     54.4      62.5
##      277      676 953     13.4       4.4      4.7     10.47     13  
##      609      344 953      8.88      2.93     3.7      6.8       8.5
##      371      582 953     12.23      3.61     4.9      9.93     11.7
##        0      953 953     66.22     11.58    29.1     58.8      67.4
##        0      953 953 253793.53 148878.18 10006   110107    240010  
##        p75     p100     hist
##       9.9      13.9 <U+2581><U+2581><U+2582><U+2587><U+2587><U+2586><U+2582><U+2581>
##       6         8.4 <U+2581><U+2581><U+2583><U+2587><U+2587><U+2583><U+2582><U+2581>
##       2.1       5.6 <U+2585><U+2587><U+2585><U+2582><U+2582><U+2581><U+2581><U+2581>
##     178    193054   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
##      69.3      83   <U+2581><U+2582><U+2583><U+2587><U+2587><U+2585><U+2582><U+2581>
##      39.27     67.4 <U+2583><U+2586><U+2587><U+2587><U+2585><U+2583><U+2581><U+2581>
##      31.7      54.8 <U+2581><U+2585><U+2587><U+2587><U+2585><U+2582><U+2581><U+2581>
##      69.8      90.7 <U+2581><U+2581><U+2582><U+2585><U+2587><U+2587><U+2585><U+2581>
##      15.93     33.2 <U+2583><U+2587><U+2587><U+2585><U+2582><U+2581><U+2581><U+2581>
##      10.5      20.7 <U+2585><U+2587><U+2587><U+2586><U+2583><U+2581><U+2581><U+2581>
##      14        35.9 <U+2583><U+2587><U+2585><U+2581><U+2581><U+2581><U+2581><U+2581>
##      75.1      94.3 <U+2581><U+2582><U+2582><U+2586><U+2587><U+2587><U+2583><U+2581>
##  360180     1e+06   <U+2587><U+2587><U+2587><U+2586><U+2581><U+2581><U+2581><U+2581>
```

## Had to use select to exclude "X17-20". They skewed my NA value. 

```r
CancerNEW <- Cancer %>%
  select(`Provider ID`, `Hospital Name`, City, State, `Number of deaths among cancer patients assigned to hospital (2010)`, `Percent of cancer patients dying in hospital (2010)`, `Percent of cancer patients admitted to hospital during the last month of life (2010 deaths)`, `Percent of cancer patients admitted to intensive care during the last month of life (2010 deaths)`, `ICU days per cancer patient during the last month of life (2010 deaths)`, `Percent of cancer patients receiving life-sustaining treatment during the last month of life (2010 deaths)`, `Percent of cancer patients receiving chemotherapy during the last two weeks of life (2010 deaths)`, `Percent of cancer patients enrolled in hospice during the last month of life (2010 deaths)`, `Percent of cancer patients enrolled in hospice during the last three days of life (2010 deaths)`, `Percent of cancer patients seeing ten or more physicians during the last six months of life (2010 deaths)`,`HIV Related` )
CancerNEW
```

```
## # A tibble: 953 x 15
##    `Provider ID` `Hospital Name` City  State `Number of deat~
##            <dbl> <chr>           <chr> <chr>            <dbl>
##  1         10006 Eliza Coffee M~ Flor~ AL                 119
##  2         10024 Jackson Hospit~ Mont~ AL                  93
##  3         10029 East Alabama M~ Opel~ AL                  86
##  4         10033 University of ~ Birm~ AL                 211
##  5         10039 Huntsville Hos~ Hunt~ AL                 331
##  6         10055 Flowers Hospit~ Doth~ AL                  81
##  7         10056 St. Vincent's ~ Birm~ AL                 102
##  8         10078 Northeast Alab~ Anni~ AL                  88
##  9         10085 Decatur Genera~ Deca~ AL                  81
## 10         10090 Providence Hos~ Mobi~ AL                  94
## # ... with 943 more rows, and 10 more variables: `Percent of cancer
## #   patients dying in hospital (2010)` <dbl>, `Percent of cancer patients
## #   admitted to hospital during the last month of life (2010
## #   deaths)` <dbl>, `Percent of cancer patients admitted to intensive care
## #   during the last month of life (2010 deaths)` <dbl>, `ICU days per
## #   cancer patient during the last month of life (2010 deaths)` <dbl>,
## #   `Percent of cancer patients receiving life-sustaining treatment during
## #   the last month of life (2010 deaths)` <dbl>, `Percent of cancer
## #   patients receiving chemotherapy during the last two weeks of life
## #   (2010 deaths)` <dbl>, `Percent of cancer patients enrolled in hospice
## #   during the last month of life (2010 deaths)` <dbl>, `Percent of cancer
## #   patients enrolled in hospice during the last three days of life (2010
## #   deaths)` <dbl>, `Percent of cancer patients seeing ten or more
## #   physicians during the last six months of life (2010 deaths)` <dbl>,
## #   `HIV Related` <chr>
```


## What are the names we are working with?

```r
names(Cancer)  
```

```
##  [1] "Provider ID"                                                                                               
##  [2] "Hospital Name"                                                                                             
##  [3] "City"                                                                                                      
##  [4] "State"                                                                                                     
##  [5] "Number of deaths among cancer patients assigned to hospital (2010)"                                        
##  [6] "Percent of cancer patients dying in hospital (2010)"                                                       
##  [7] "Percent of cancer patients admitted to hospital during the last month of life (2010 deaths)"               
##  [8] "Hospital days per cancer patient during the last month of life (2010 deaths)"                              
##  [9] "Percent of cancer patients admitted to intensive care during the last month of life (2010 deaths)"         
## [10] "ICU days per cancer patient during the last month of life (2010 deaths)"                                   
## [11] "Percent of cancer patients receiving life-sustaining treatment during the last month of life (2010 deaths)"
## [12] "Percent of cancer patients receiving chemotherapy during the last two weeks of life (2010 deaths)"         
## [13] "Percent of cancer patients enrolled in hospice during the last month of life (2010 deaths)"                
## [14] "Hospice days per cancer patient during the last month of life (2010 deaths)"                               
## [15] "Percent of cancer patients enrolled in hospice during the last three days of life (2010 deaths)"           
## [16] "Percent of cancer patients seeing ten or more physicians during the last six months of life (2010 deaths)" 
## [17] "HIV Related"
```
#these names aren't great

## Renaming the data 

```r
CancerName <- CancerNEW %>%
dplyr::rename( Deaths_in_hospital = "Number of deaths among cancer patients assigned to hospital (2010)", 
               Percent_death = "Percent of cancer patients dying in hospital (2010)", 
percent_admitted_LM = "Percent of cancer patients admitted to hospital during the last month of life (2010 deaths)", 
percent_admitted_ICU_LM = "Percent of cancer patients admitted to intensive care during the last month of life (2010 deaths)", 
Avg_ICU_Days_LM = "ICU days per cancer patient during the last month of life (2010 deaths)", 
Percent_receiving_LStreatment_LM = "Percent of cancer patients receiving life-sustaining treatment during the last month of life (2010 deaths)", 
Percent_receiving_chemo_L2W = "Percent of cancer patients receiving chemotherapy during the last two weeks of life (2010 deaths)", 
Percent_Hospice_LM = "Percent of cancer patients enrolled in hospice during the last month of life (2010 deaths)", 
Percent_Hospice_L3D = "Percent of cancer patients enrolled in hospice during the last three days of life (2010 deaths)", 
Percent_seeing_10ormoreMDs_L6M = "Percent of cancer patients seeing ten or more physicians during the last six months of life (2010 deaths)")
CancerName
```

```
## # A tibble: 953 x 15
##    `Provider ID` `Hospital Name` City  State Deaths_in_hospi~ Percent_death
##            <dbl> <chr>           <chr> <chr>            <dbl>         <dbl>
##  1         10006 Eliza Coffee M~ Flor~ AL                 119          42  
##  2         10024 Jackson Hospit~ Mont~ AL                  93          29.5
##  3         10029 East Alabama M~ Opel~ AL                  86          24.7
##  4         10033 University of ~ Birm~ AL                 211          34.3
##  5         10039 Huntsville Hos~ Hunt~ AL                 331          36.1
##  6         10055 Flowers Hospit~ Doth~ AL                  81          29.3
##  7         10056 St. Vincent's ~ Birm~ AL                 102          29.6
##  8         10078 Northeast Alab~ Anni~ AL                  88          40.3
##  9         10085 Decatur Genera~ Deca~ AL                  81          18.3
## 10         10090 Providence Hos~ Mobi~ AL                  94          35.7
## # ... with 943 more rows, and 9 more variables: percent_admitted_LM <dbl>,
## #   percent_admitted_ICU_LM <dbl>, Avg_ICU_Days_LM <dbl>,
## #   Percent_receiving_LStreatment_LM <dbl>,
## #   Percent_receiving_chemo_L2W <dbl>, Percent_Hospice_LM <dbl>,
## #   Percent_Hospice_L3D <dbl>, Percent_seeing_10ormoreMDs_L6M <dbl>, `HIV
## #   Related` <chr>
```
## These are much better

```r
names(CancerName)
```

```
##  [1] "Provider ID"                      "Hospital Name"                   
##  [3] "City"                             "State"                           
##  [5] "Deaths_in_hospital"               "Percent_death"                   
##  [7] "percent_admitted_LM"              "percent_admitted_ICU_LM"         
##  [9] "Avg_ICU_Days_LM"                  "Percent_receiving_LStreatment_LM"
## [11] "Percent_receiving_chemo_L2W"      "Percent_Hospice_LM"              
## [13] "Percent_Hospice_L3D"              "Percent_seeing_10ormoreMDs_L6M"  
## [15] "HIV Related"
```


## How many NAs are in our data?

```r
CancerName %>%
  summarize(number_na=sum(is.na(CancerName)))
```

```
## # A tibble: 1 x 1
##   number_na
##       <int>
## 1      1303
```

## Where are these NAs Located?

```r
  CancerName %>%
  purrr::map_df(~sum(is.na(.))) %>%
  tidyr::gather(key="variables", value="num_nas") %>% 
  arrange(desc(num_nas))
```

```
## # A tibble: 15 x 2
##    variables                        num_nas
##    <chr>                              <int>
##  1 Percent_receiving_chemo_L2W          609
##  2 Percent_receiving_LStreatment_LM     371
##  3 Percent_Hospice_L3D                  277
##  4 percent_admitted_ICU_LM               15
##  5 Avg_ICU_Days_LM                       15
##  6 Percent_death                         12
##  7 City                                   2
##  8 State                                  2
##  9 Provider ID                            0
## 10 Hospital Name                          0
## 11 Deaths_in_hospital                     0
## 12 percent_admitted_LM                    0
## 13 Percent_Hospice_LM                     0
## 14 Percent_seeing_10ormoreMDs_L6M         0
## 15 HIV Related                            0
```

## We have a ton of NAs. Majority of these come from the the variables shown in the previous slide.

## Highest death rate by State

```r
CancerName %>%
  select(State, Percent_death) %>%
  arrange(desc(Percent_death))
```

```
## # A tibble: 953 x 2
##    State Percent_death
##    <chr>         <dbl>
##  1 NY             54.8
##  2 NY             52.8
##  3 NJ             52.4
##  4 IN             51.7
##  5 NY             49.8
##  6 NY             49.8
##  7 CA             49.7
##  8 CA             48.6
##  9 NY             48.6
## 10 NY             48.1
## # ... with 943 more rows
```
##Percent Death by Top 3 States

```r
CancerName %>%
  group_by(State) %>% 
  filter(State %in% c("NY", "NJ", "IN")) %>%
  ggplot(aes(x=State, y=Percent_death))+
  geom_boxplot()+
  labs(title = "Percent Death by Top 3 States",
       y = "% Death")+
  theme(plot.title = element_text(size = rel(1.5), hjust = 0.5))
```

##Boxplot
![](Project_Presentation_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

## Lowest death rate by State

```r
CancerName %>%
  select(State, Percent_death) %>%
  arrange(Percent_death)
```

```
## # A tibble: 953 x 2
##    State Percent_death
##    <chr>         <dbl>
##  1 FL              7  
##  2 FL              8.9
##  3 FL              9.6
##  4 FL             11.1
##  5 NJ             11.2
##  6 MI             11.3
##  7 FL             11.4
##  8 PA             11.5
##  9 TN             11.6
## 10 FL             11.7
## # ... with 943 more rows
```

##Percent Death by Top and Bottom States

```r
CancerName %>%
  group_by(State) %>% 
  filter(State %in% c("NY", "FL")) %>%
  ggplot(aes(x=State, y=Percent_death))+
  geom_boxplot()+
  labs(title = "Percent Death by Top and Bottom States",
       y = "% Death")+
  theme(plot.title = element_text(size = rel(1.5), hjust = 0.5))
```

##Boxplot
![](Project_Presentation_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

## Focusing strictly on HIV related cases 

```r
CancerHIV <- CancerName %>%
  filter(`HIV Related`=="HIV")
CancerHIV
```

```
## # A tibble: 128 x 15
##    `Provider ID` `Hospital Name` City  State Deaths_in_hospi~ Percent_death
##            <dbl> <chr>           <chr> <chr>            <dbl>         <dbl>
##  1         10033 University of ~ Birm~ AL                 211          34.3
##  2         30064 University Med~ Tucs~ AZ                 131          18.3
##  3         30103 Mayo Clinic Ho~ Phoe~ AZ                 206          13.1
##  4         40016 UAMS Medical C~ Litt~ AR                 145          27.5
##  5         50025 UC San Diego H~ San ~ CA                 117          27.8
##  6         50146 City of Hope-H~ Duar~ CA                 118          24.2
##  7         50262 Ronald Reagan ~ Los ~ CA                 109          40  
##  8         50327 Loma Linda Uni~ Loma~ CA                 102          31.3
##  9         50348 UC Irvine Heal~ Oran~ CA                  95          29.9
## 10         50441 Stanford Hospi~ Stan~ CA                 200          36.6
## # ... with 118 more rows, and 9 more variables: percent_admitted_LM <dbl>,
## #   percent_admitted_ICU_LM <dbl>, Avg_ICU_Days_LM <dbl>,
## #   Percent_receiving_LStreatment_LM <dbl>,
## #   Percent_receiving_chemo_L2W <dbl>, Percent_Hospice_LM <dbl>,
## #   Percent_Hospice_L3D <dbl>, Percent_seeing_10ormoreMDs_L6M <dbl>, `HIV
## #   Related` <chr>
```

## What's the average deaths per state for HIV cases 

```r
CancerHIV_2 <-CancerHIV %>%
  select(City, State, Deaths_in_hospital) %>%
  filter(!State=="NA") %>%
  group_by(State) %>%
  summarize(
    State_mean = mean(Deaths_in_hospital)) %>%
  arrange(desc(State_mean))
CancerHIV_2
```

```
## # A tibble: 42 x 2
##    State State_mean
##    <chr>      <dbl>
##  1 IL          263.
##  2 IN          262 
##  3 MO          262.
##  4 TX          246 
##  5 NY          239.
##  6 ME          238 
##  7 NC          237.
##  8 OH          233 
##  9 CT          216 
## 10 MI          215.
## # ... with 32 more rows
```

## How does this look on a plot? 

```r
CancerHIV_2 %>% 
  top_n(10) %>%
  ggplot(aes(x=reorder(State, State_mean), y=State_mean, fill=State)) +
  geom_bar(stat = "identity", alpha=1) +
  labs(title = "Number of Cancer-related deaths caused by HIV (State)", 
       x= "State", 
       y= " Numner of deaths") +
  theme_bw() +
  theme(panel.grid.major = element_line(colour = "gray 50")) +
   theme(plot.title = element_text(size = rel(1.5), hjust= 0.5)) 
```
## How does this look on a plot? 

```
## Selecting by State_mean
```

![](Project_Presentation_files/figure-html/unnamed-chunk-22-1.png)<!-- -->


## Does that mean IL, or competing states, are the worst state to treat cancer cases caused by HIV?

```r
CancerHIV_3<- CancerHIV %>%
  select(City, State, Percent_death, `HIV Related`) %>% 
  group_by(State) %>%
  summarize(State_mean_percent_death=mean(Percent_death))
CancerHIV_3
```

```
## # A tibble: 43 x 2
##    State State_mean_percent_death
##    <chr>                    <dbl>
##  1 AL                        34.3
##  2 AR                        27.5
##  3 AZ                        15.7
##  4 CA                        33.9
##  5 CO                        17.3
##  6 CT                        32.9
##  7 DC                        40  
##  8 FL                        27.0
##  9 GA                        23.4
## 10 IA                        20.3
## # ... with 33 more rows
```
## Let's see

```r
CancerHIV_3 %>%
  top_n(10) %>%
  ggplot(aes(x=reorder(State,State_mean_percent_death), y= State_mean_percent_death, fill=State)) +
  geom_bar(stat = "identity", alpha=1) +
    labs(title = "Percent death of admitted HIV patients (State)", 
       x= "State", 
       y= " Percent of death") + 
  theme_bw() +
    theme(panel.grid.major = element_line(colour = "gray 50")) +
   theme(plot.title = element_text(size = rel(1.5), hjust= 0.5)) 
```

## Let's see

```
## Selecting by State_mean_percent_death
```

![](Project_Presentation_files/figure-html/unnamed-chunk-25-1.png)<!-- -->

## Here we clearly see that the top 10 of previous plot are drastically different from these


## Let's see how the top 5 states from the previous plot compare to their "Regular" case's data?

```r
CancerHIV_3 <- CancerHIV_3 %>%
   filter(State== "DC"| State== "OK" |State== "NY" |State== "AL"| State== "CA" ) 

CancerHIV_3$HIV_related<-c('HIV','HIV','HIV','HIV','HIV')
CancerHIV_3
```

```
## # A tibble: 5 x 3
##   State State_mean_percent_death HIV_related
##   <chr>                    <dbl> <chr>      
## 1 AL                        34.3 HIV        
## 2 CA                        33.9 HIV        
## 3 DC                        40   HIV        
## 4 NY                        36.6 HIV        
## 5 OK                        39.4 HIV
```
## Continued

```r
  Cancer_reg_mean<- CancerName %>%
  filter(!`HIV Related`=="HIV") %>%
  select(City, State, Percent_death) %>% 
  group_by(State) %>%
  summarize(State_mean_percent_death=mean(Percent_death)) %>%
    filter(State== "DC"| State== "OK" |State== "NY" |State== "AL"| State== "CA" )
Cancer_reg_mean
```

```
## # A tibble: 5 x 2
##   State State_mean_percent_death
##   <chr>                    <dbl>
## 1 AL                        31.5
## 2 CA                        32.4
## 3 DC                        36.5
## 4 NY                        36.1
## 5 OK                        31.6
```
## Continued

```r
Cancer_reg_mean$HIV_related<-c('N-HIV','N-HIV','N-HIV','N-HIV','N-HIV')
Cancer_reg_mean
```

```
## # A tibble: 5 x 3
##   State State_mean_percent_death HIV_related
##   <chr>                    <dbl> <chr>      
## 1 AL                        31.5 N-HIV      
## 2 CA                        32.4 N-HIV      
## 3 DC                        36.5 N-HIV      
## 4 NY                        36.1 N-HIV      
## 5 OK                        31.6 N-HIV
```
## Continued

```r
Cancer_matix <- rbind(Cancer_reg_mean,CancerHIV_3) 
Cancer_matix
```

```
## # A tibble: 10 x 3
##    State State_mean_percent_death HIV_related
##    <chr>                    <dbl> <chr>      
##  1 AL                        31.5 N-HIV      
##  2 CA                        32.4 N-HIV      
##  3 DC                        36.5 N-HIV      
##  4 NY                        36.1 N-HIV      
##  5 OK                        31.6 N-HIV      
##  6 AL                        34.3 HIV        
##  7 CA                        33.9 HIV        
##  8 DC                        40   HIV        
##  9 NY                        36.6 HIV        
## 10 OK                        39.4 HIV
```
## HIV-Death percents vs Regular-death percents (States)

```r
Cancer_matix %>%
  ggplot(aes(x=reorder(State,State_mean_percent_death), y= State_mean_percent_death, fill= `HIV_related` )) +
  geom_bar(stat = "identity",position = "dodge") +
  labs(title = "HIV-Death percents vs Regular-death percents (States)", 
       x= "State", 
       y= "Death Percent") +
  theme_bw() +
  theme(panel.grid.major = element_line(colour = "gray 50")) +
   theme(plot.title = element_text(size = rel(1.5), hjust= 0.5)) +
  scale_fill_discrete(name = "HIV Related")
```
## Bar Graph
![](Project_Presentation_files/figure-html/unnamed-chunk-31-1.png)<!-- -->
