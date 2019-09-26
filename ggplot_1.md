ggplot\_1
================
Clement Mugenzi
9/26/2019

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(c("USW00094728", "USC00519397", "USS0023B17S"),
                      var = c("PRCP", "TMIN", "TMAX"), 
                      date_min = "2017-01-01",
                      date_max = "2017-12-31") %>%
  mutate(
    name = recode(id, USW00094728 = "CentralPark_NY", 
                      USC00519397 = "Waikiki_HA",
                      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'crul':
    ##   method                 from
    ##   as.character.form_file httr

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## file path:          /Users/clementmugenzi/Library/Caches/rnoaa/ghcnd/USW00094728.dly

    ## file last updated:  2019-09-03 20:39:20

    ## file min/max dates: 1869-01-01 / 2019-09-30

    ## file path:          /Users/clementmugenzi/Library/Caches/rnoaa/ghcnd/USC00519397.dly

    ## file last updated:  2019-09-03 20:39:33

    ## file min/max dates: 1965-01-01 / 2019-09-30

    ## file path:          /Users/clementmugenzi/Library/Caches/rnoaa/ghcnd/USS0023B17S.dly

    ## file last updated:  2019-09-03 20:39:37

    ## file min/max dates: 1999-09-01 / 2019-09-30

``` r
view(weather_df)
```

## Create ggplot

``` r
ggplot(weather_df, aes(x = tmin, y = tmax)) +
  geom_point()
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot_1_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

alternative way of making this plot

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point()
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot_1_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

saving initial plot: mostly i dont use this

``` r
scatterplot = 
  weather_df %>% 
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point()

scatterplot
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot_1_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

adding color

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax)) +
  geom_point(aes(color = name), alpha = .4) # alpha is for transparancy
```

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot_1_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

why do `aes` positions matter?

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = .4) +
  geom_smooth(se = F) # se makes the plot cleaner
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 15 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot_1_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

facets

``` r
weather_df %>% 
  ggplot(aes(x = tmin, y = tmax, color = name)) +
  geom_point(alpha = .4) +
  geom_smooth(se = F) +
  facet_grid(~name)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 15 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 15 rows containing missing values (geom_point).

![](ggplot_1_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

this is fine. but not interesting

``` r
weather_df %>% 
  ggplot(aes(x = date, y = tmax, color = name)) +
  geom_point(aes(size = prcp), alpha = .5) +
  geom_smooth(size = 2, se = F)
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

    ## Warning: Removed 3 rows containing non-finite values (stat_smooth).

    ## Warning: Removed 3 rows containing missing values (geom_point).

![](ggplot_1_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

## Some extra stuff
