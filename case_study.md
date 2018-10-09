Case Study
================
Jeff Goldsmith
10/9/2018

Load the data.

``` r
library(p8105.datasets)
data(nyc_airbnb)
```

Data cleaning - Remove/clean some things

``` r
nyc_airbnb <- nyc_airbnb %>% 
  mutate(stars = review_scores_location / 2) %>% 
  rename(boro = neighbourhood_group)

nyc_airbnb %>% 
  count(boro)
```

    ## # A tibble: 5 x 2
    ##   boro              n
    ##   <chr>         <int>
    ## 1 Bronx           649
    ## 2 Brooklyn      16810
    ## 3 Manhattan     19212
    ## 4 Queens         3821
    ## 5 Staten Island   261

``` r
nyc_airbnb %>% 
  count(boro, neighbourhood) %>% 
  View
```

Some questions
--------------

-   Does rating differ by room type, neighborhood, or both?
-   How does price relate to other variables?
-   Where are the locations of rentals? Do locations differ by rental type?
-   Where are the cheapest places? Most expensive?
-   Most unfilled days?
-   Which area has highest density?
-   Host characteristics?

How many ppl have more than 1 listing?

``` r
nyc_airbnb %>% 
  count(host_id, boro) %>% 
  filter(n > 1) %>% 
  count(boro)
```

    ## # A tibble: 5 x 2
    ##   boro             nn
    ##   <chr>         <int>
    ## 1 Bronx            79
    ## 2 Brooklyn       1770
    ## 3 Manhattan      1547
    ## 4 Queens          466
    ## 5 Staten Island    40

Average rating by neighborhood?

``` r
nyc_airbnb %>% 
  group_by(neighbourhood, boro) %>% 
  summarize(avg_rate = mean(stars, na.rm = T)) %>% 
  arrange(desc(avg_rate)) %>% 
  ungroup() %>% 
  group_by(boro) %>% 
  summarize(boro_rate = mean(avg_rate, na.rm = T)) 
```

    ## # A tibble: 5 x 2
    ##   boro          boro_rate
    ##   <chr>             <dbl>
    ## 1 Bronx              4.46
    ## 2 Brooklyn           4.64
    ## 3 Manhattan          4.80
    ## 4 Queens             4.57
    ## 5 Staten Island      4.69
