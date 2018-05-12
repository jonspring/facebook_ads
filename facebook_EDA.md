Exploratory Data Analysis of facebook ads
================

``` r
library(tidyverse, quietly = TRUE); library(readr)
FacebookAds <- read_csv("FacebookAds.csv")
```

Let's have a histogram party and look at some of the fields with numbers and categories.

``` r
FacebookAds_quant <-
  FacebookAds %>%
  select(AdID, AdSpend, Impressions, Clicks, CreationDate,
         EndDate) %>%
  mutate(CreationDate = lubridate::ymd_hms(CreationDate) %>%
           lubridate::decimal_date(),
         EndDate = lubridate::ymd_hms(EndDate) %>%
           lubridate::decimal_date()) %>%
  gather(stat, value, -AdID)
```

    ## Warning: 2059 failed to parse.

    ## Warning: 1359 failed to parse.

``` r
ggplot(FacebookAds_quant, aes(value)) +
  geom_histogram(bins = 100) +
  facet_wrap(~stat, scales = "free")
```

    ## Warning: Removed 6289 rows containing non-finite values (stat_bin).

![](facebook_EDA_files/figure-markdown_github/unnamed-chunk-2-1.png) Spend vs. Clicks vs. Impressons

``` r
# FacebookAds_quant %>% 
#   spread(stat, value) %>%
#   mutate(clicks_per_spend = Clicks / AdSpend,
#          impr_per_spend   = Impressions / AdSpend)
#   ggplot(aes(clicks_per_spend, impr_per_spend)) +
#   geom_point()
```