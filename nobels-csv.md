Nobel winners
================
Kevin Diegel

``` r
library(tidyverse)
library(readr)
library(readxl)
library(knitr)
library(janitor)
```

``` r
knitr::opts_chunk$set(error = TRUE, message = TRUE)
```

Let’s first load the data:

``` r
nobel <- read.csv("./data-raw/nobel.csv")
```

Then let’s split the data into two:

``` r
nobelstem <- nobel %>%
  filter(category %in% c("Physics", "Chemistry", "Medicine", "Economics"))
```

``` r
nonstem <- nobel %>%
  filter(!category %in% c("Physics", "Chemistry", "Medicine", "Economics"))
```

    ## Error in filter(., !category %in% c("Physics", "Chemistry", "Medicine", : object 'nobel' not found

And finally write out the data:

``` r
write.csv(nobelstem, file = "./data/nobelstem.csv")
```

    ## Error in is.data.frame(x): object 'nobelstem' not found

``` r
write.csv(nonstem, file ="./data/nonstem.csv")
```

    ## Error in is.data.frame(x): object 'nonstem' not found

Sales:

``` r
sales <- read_excel(path = "./data-raw/sales.xlsx", skip = 3, col_names = c("id", "n"))
```

``` r
sales %>%
mutate(is_brand_name = str_detect(id, "Brand")) %>%
mutate(brand = if_else(is_brand_name, id, NULL)) %>%
fill(brand) %>%
filter(!is_brand_name)%>%
select(brand, id, n)
```

    ## # A tibble: 7 × 3
    ##   brand   id    n    
    ##   <chr>   <chr> <chr>
    ## 1 Brand 1 1234  8    
    ## 2 Brand 1 8721  2    
    ## 3 Brand 1 1822  3    
    ## 4 Brand 2 3333  1    
    ## 5 Brand 2 2156  3    
    ## 6 Brand 2 3987  6    
    ## 7 Brand 2 3216  5
