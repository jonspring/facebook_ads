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
FacebookAds %>%
  ggplot(aes(AdSpend, Impressions, color = log(Clicks), 
             label = str_wrap(str_sub(AdText, end = 120)))) +
  geom_point(size = 1/2, alpha = 1/2) +
  scale_x_continuous(trans = "log10", labels = scales::comma) +
  scale_y_continuous(trans = "log10", labels = scales::comma) +
  ggrepel::geom_text_repel(data = FacebookAds %>% top_n(5, Impressions), size = 2)
```

    ## Warning: Transformation introduced infinite values in continuous y-axis

    ## Warning: Removed 1054 rows containing missing values (geom_point).

    ## Warning: Removed 1 rows containing missing values (geom_text_repel).

![](facebook_EDA_files/figure-markdown_github/unnamed-chunk-3-1.png)
