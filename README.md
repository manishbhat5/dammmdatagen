
<!-- README.md is generated from README.Rmd. Please edit that file -->
dammmdatagen
============

The goal of dammmdatagen is to make it easy for marketing mix modeling professionals to get access to realistic data sets where the ground truth is known. This fascilitates our development and provides in the end more value for all stakeholders of MMM.

Installation
------------

You can install dammmdatagen from github with:

``` r
# install.packages("devtools")
devtools::install_github("DoktorMike/dammmdatagen")
```

Quick start
-----------

This is a basic example which shows you how to generate a small 1 year data set.

``` r
# load useful libraries
library(dammmdatagen)

# generate a basic data set
mydf <- generateData()
head(mydf)
#> # A tibble: 6 x 10
#>   date       sunshine precip… temp… compe… comp… compe…   cpi    cci   gdp
#>   <date>        <dbl>   <dbl> <dbl>  <dbl> <dbl>  <dbl> <dbl>  <dbl> <dbl>
#> 1 2017-01-14   -1.65   -0.199 0.859 100000 70000 100000  0      0     0   
#> 2 2017-01-15   -1.98   -1.73  0.811  70000     0  20000 -1.46 - 2.45  2.71
#> 3 2017-01-16   -2.21   -2.04  2.27   30000 20000  40000 -2.67 - 5.17  5.55
#> 4 2017-01-17   -1.52   -1.52  1.84       0 40000  80000 -4.12 - 8.11  9.76
#> 5 2017-01-18   -1.34   -1.63  2.21   30000     0  70000 -3.18 -11.0  14.7 
#> 6 2017-01-19    0.263  -0.162 3.01   30000 20000  80000 -1.76 -13.9  16.8
```

We can do a lot more of course! In this small snippet we'll generate 1 month worth of competitor media spendings data and plot that out.

``` r
library(dammmdatagen)
library(ggplot2)
library(dplyr)
library(tidyr)
library(scales)

generateCompetitorData(fromDate = Sys.Date()-30, toDate = Sys.Date()) %>% 
  gather("competitor", "spend", -"date") %>% 
  ggplot(aes(y=spend, x=date, fill=competitor)) + 
  geom_bar(stat="identity", position = position_stack()) + 
  theme_minimal() + scale_y_continuous(labels = dollar_format(prefix = "kr. "))
```

![](README-competitorspendplot-1.png)

Just as we can generate competitor spending data we can also generate macroeconomical data. These types of indicates are typically slow moving over time with minor temporal differences.

``` r
generateMacroData(fromDate = Sys.Date()-30, toDate = Sys.Date()) %>% 
  gather("indicator", "value", -"date") %>% 
  ggplot(aes(y=value, x=date, color=indicator)) + 
  geom_line(size = 1.5) + theme_minimal()
```

![](README-macroecondataplot-1.png)
