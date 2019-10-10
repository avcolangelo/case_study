Case Study
================
Alexis
10/10/2019

``` r
library(p8105.datasets)
data("nyc_airbnb")
nyc_airbnb %>% view
```

## let’s answer some questions

some cleaning

``` r
nyc_airbnb =
  nyc_airbnb %>%
  mutate(
    stars = review_scores_location / 2,
    borough = neighbourhood_group
  )
```

``` r
nyc_airbnb %>%
  group_by(borough) %>%
  summarize(med_price = median(price, na.rm = TRUE))
```

    ## # A tibble: 5 x 2
    ##   borough       med_price
    ##   <chr>             <dbl>
    ## 1 Bronx                65
    ## 2 Brooklyn             90
    ## 3 Manhattan           137
    ## 4 Queens               75
    ## 5 Staten Island        75

``` r
nyc_airbnb %>%
  group_by(borough, room_type) %>%
  summarize(med_price = median(price, na.rm = TRUE))
```

    ## # A tibble: 15 x 3
    ## # Groups:   borough [5]
    ##    borough       room_type       med_price
    ##    <chr>         <chr>               <dbl>
    ##  1 Bronx         Entire home/apt      100 
    ##  2 Bronx         Private room          55 
    ##  3 Bronx         Shared room           43 
    ##  4 Brooklyn      Entire home/apt      145 
    ##  5 Brooklyn      Private room          65 
    ##  6 Brooklyn      Shared room           40 
    ##  7 Manhattan     Entire home/apt      190 
    ##  8 Manhattan     Private room          90 
    ##  9 Manhattan     Shared room           65 
    ## 10 Queens        Entire home/apt      119 
    ## 11 Queens        Private room          60 
    ## 12 Queens        Shared room           39 
    ## 13 Staten Island Entire home/apt      112.
    ## 14 Staten Island Private room          55 
    ## 15 Staten Island Shared room           25

``` r
nyc_airbnb %>%
  group_by(borough, room_type) %>%
  summarize(med_price = median(price, na.rm = TRUE)) %>%
  pivot_wider(
    names_from = room_type,
    values_from = med_price
  )
```

    ## # A tibble: 5 x 4
    ## # Groups:   borough [5]
    ##   borough       `Entire home/apt` `Private room` `Shared room`
    ##   <chr>                     <dbl>          <dbl>         <dbl>
    ## 1 Bronx                      100              55            43
    ## 2 Brooklyn                   145              65            40
    ## 3 Manhattan                  190              90            65
    ## 4 Queens                     119              60            39
    ## 5 Staten Island              112.             55            25

``` r
nyc_airbnb %>%
  filter(borough == "Staten Island", room_type == "Shared room") %>% view
```

``` r
nyc_airbnb %>%
  count(borough, room_type) %>%
  pivot_wider(
    names_from = room_type,
    values_from = n
  )
```

    ## # A tibble: 5 x 4
    ##   borough       `Entire home/apt` `Private room` `Shared room`
    ##   <chr>                     <int>          <int>         <int>
    ## 1 Bronx                       192            429            28
    ## 2 Brooklyn                   7427           9000           383
    ## 3 Manhattan                 10814           7812           586
    ## 4 Queens                     1388           2241           192
    ## 5 Staten Island               116            144             1
