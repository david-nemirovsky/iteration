Iteration and List Columns
================

## Lists

You can put anything in a list.

``` r
l = list(
  vec_numeric = 5:8, 
  vec_logical = c(TRUE, TRUE, FALSE, TRUE, FALSE, FALSE), 
  mat = matrix(1:8, nrow = 2, ncol = 4), 
  summary = summary(rnorm(100))
)
```

``` r
l
```

    ## $vec_numeric
    ## [1] 5 6 7 8
    ## 
    ## $vec_logical
    ## [1]  TRUE  TRUE FALSE  TRUE FALSE FALSE
    ## 
    ## $mat
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8
    ## 
    ## $summary
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ## -2.1850 -0.8152 -0.2012 -0.1297  0.5084  1.6280

``` r
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[1]]
```

    ## [1] 5 6 7 8

``` r
mean(l[["vec_numeric"]])
```

    ## [1] 6.5

## `for` Loop

Create a new list:

``` r
list_norm = 
  list(
    a = rnorm(20, mean = 3, sd = 1), 
    b = rnorm(20, mean = 0, sd = 5), 
    c = rnorm(20, mean = 10, sd = 0.2), 
    d = rnorm(20, mean = -3, sd = 1)
  )
```

``` r
list_norm
```

    ## $a
    ##  [1] 2.489719 3.061676 3.097624 3.441006 3.767400 1.955086 4.409490 3.478273
    ##  [9] 3.023231 3.851498 3.733716 4.118125 3.790364 3.701978 3.352661 2.763107
    ## [17] 1.916466 1.806309 2.875211 3.014670
    ## 
    ## $b
    ##  [1] -8.3462132 -5.4169568  4.9348368  0.6819236 -8.5358518  2.3055035
    ##  [7] -0.8458151 -0.7849457  2.2407670  7.4977769  1.9719256 -0.7910904
    ## [13] -3.7014842  2.5432190  0.9732456  1.7814593 -1.1840007  8.6453932
    ## [19]  2.1859494 -7.8266287
    ## 
    ## $c
    ##  [1]  9.979682 10.138624  9.768926 10.004393  9.783369 10.005558 10.066459
    ##  [8] 10.264770 10.047902 10.071147 10.126908 10.128289 10.131833 10.172020
    ## [15]  9.872506 10.013408 10.227300 10.048622  9.705540  9.982214
    ## 
    ## $d
    ##  [1] -2.883760 -2.227895 -2.198230 -4.810757 -5.267750 -2.475201 -3.049914
    ##  [8] -2.723072 -4.118109 -3.112686 -1.522294 -2.923168 -1.505686 -3.878825
    ## [15] -3.150089 -2.198042 -2.584703 -1.488822 -2.021354 -3.807847

Pause and get my old function

``` r
mean_and_sd = function(x){
  
  if(!is.numeric(x)) {
    stop("Input must be numeric")
  }
  
  if(length(x) < 3) {
    stop("Input must have at least 3 numbers")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)
  
  tibble(
    mean = mean_x,
    sd = sd_x
  )
  
}
```

I can apply that function to each list element:

``` r
mean_and_sd(list_norm[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.18 0.730

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##      mean    sd
    ##     <dbl> <dbl>
    ## 1 -0.0835  4.79

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.149

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.90  1.05

Let’s use a `for` loop:

``` r
output = vector("list", length = 4)

for(i in 1:4) {

  output[[i]] = mean_and_sd(list_norm[[i]])

}
```

## Let’s try map\!\!

``` r
output = map(list_norm, mean_and_sd)
```

What if you want a different function?

``` r
output = map(list_norm, median)
```

``` r
output = map_dbl(list_norm, median, id = "input")
```

``` r
output = map_df(list_norm, mean_and_sd, .id = "input")
```

## List columns\!

``` r
listcol_df = 
  tibble(
    name = c("a", "b", "c", "d"),
    samp = list_norm
  )
```

``` r
listcol_df %>% 
  pull(name)
```

    ## [1] "a" "b" "c" "d"

``` r
listcol_df %>% 
  pull(samp)
```

    ## $a
    ##  [1] 2.489719 3.061676 3.097624 3.441006 3.767400 1.955086 4.409490 3.478273
    ##  [9] 3.023231 3.851498 3.733716 4.118125 3.790364 3.701978 3.352661 2.763107
    ## [17] 1.916466 1.806309 2.875211 3.014670
    ## 
    ## $b
    ##  [1] -8.3462132 -5.4169568  4.9348368  0.6819236 -8.5358518  2.3055035
    ##  [7] -0.8458151 -0.7849457  2.2407670  7.4977769  1.9719256 -0.7910904
    ## [13] -3.7014842  2.5432190  0.9732456  1.7814593 -1.1840007  8.6453932
    ## [19]  2.1859494 -7.8266287
    ## 
    ## $c
    ##  [1]  9.979682 10.138624  9.768926 10.004393  9.783369 10.005558 10.066459
    ##  [8] 10.264770 10.047902 10.071147 10.126908 10.128289 10.131833 10.172020
    ## [15]  9.872506 10.013408 10.227300 10.048622  9.705540  9.982214
    ## 
    ## $d
    ##  [1] -2.883760 -2.227895 -2.198230 -4.810757 -5.267750 -2.475201 -3.049914
    ##  [8] -2.723072 -4.118109 -3.112686 -1.522294 -2.923168 -1.505686 -3.878825
    ## [15] -3.150089 -2.198042 -2.584703 -1.488822 -2.021354 -3.807847

Let’s try some operations:

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.18 0.730

``` r
mean_and_sd(listcol_df$samp[[2]])
```

    ## # A tibble: 1 x 2
    ##      mean    sd
    ##     <dbl> <dbl>
    ## 1 -0.0835  4.79

Can I just map??

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.18 0.730
    ## 
    ## $b
    ## # A tibble: 1 x 2
    ##      mean    sd
    ##     <dbl> <dbl>
    ## 1 -0.0835  4.79
    ## 
    ## $c
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.149
    ## 
    ## $d
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.90  1.05

So, can I add a list column??

``` r
listcol_df = 
  listcol_df %>% 
    mutate(summary = map(samp, mean_and_sd),
           medians = map_dbl(samp, median))
```

## Weather Data

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USC00519397", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2017-01-01",
    date_max = "2017-12-31") %>%
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Waikiki_HA",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## using cached file: C:\Users\David\AppData\Local\Cache/R/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2020-10-08 00:19:38 (7.54)

    ## file min/max dates: 1869-01-01 / 2020-10-31

    ## using cached file: C:\Users\David\AppData\Local\Cache/R/noaa_ghcnd/USC00519397.dly

    ## date created (size, mb): 2020-10-08 00:20:21 (1.703)

    ## file min/max dates: 1965-01-01 / 2020-03-31

    ## using cached file: C:\Users\David\AppData\Local\Cache/R/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2020-10-08 00:20:48 (0.882)

    ## file min/max dates: 1999-09-01 / 2020-10-31

Get our list columns:

``` r
weather_nest = 
  weather_df %>% 
  nest(data = date:tmin)
```

``` r
weather_nest %>% pull(name)
```

    ## [1] "CentralPark_NY" "Waikiki_HA"     "Waterhole_WA"

``` r
weather_nest %>% pull(data)
```

    ## [[1]]
    ## # A tibble: 365 x 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # ... with 355 more rows
    ## 
    ## [[2]]
    ## # A tibble: 365 x 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0  26.7  16.7
    ##  2 2017-01-02     0  27.2  16.7
    ##  3 2017-01-03     0  27.8  17.2
    ##  4 2017-01-04     0  27.2  16.7
    ##  5 2017-01-05     0  27.8  16.7
    ##  6 2017-01-06     0  27.2  16.7
    ##  7 2017-01-07     0  27.2  16.7
    ##  8 2017-01-08     0  25.6  15  
    ##  9 2017-01-09     0  27.2  15.6
    ## 10 2017-01-10     0  28.3  17.2
    ## # ... with 355 more rows
    ## 
    ## [[3]]
    ## # A tibble: 365 x 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01   432  -6.8 -10.7
    ##  2 2017-01-02    25 -10.5 -12.4
    ##  3 2017-01-03     0  -8.9 -15.9
    ##  4 2017-01-04     0  -9.9 -15.5
    ##  5 2017-01-05     0  -5.9 -14.2
    ##  6 2017-01-06     0  -4.4 -11.3
    ##  7 2017-01-07    51   0.6 -11.5
    ##  8 2017-01-08    76   2.3  -1.2
    ##  9 2017-01-09    51  -1.2  -7  
    ## 10 2017-01-10     0  -5   -14.2
    ## # ... with 355 more rows

Suppose we want to regress ‘tmax’ on ‘tmin’ for each station?

This works….

``` r
lm(tmax ~ tmin, data = weather_nest$data[[1]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nest$data[[1]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039

Let’s write a function to do this regression:

``` r
weather_lm = function(df){
  
  lm(tmax ~ tmin, data = df)
  
}

output = vector("list", 3)

for(i in 1:3){
  
  output[[i]] = weather_lm(weather_nest$data[[i]])
  
}

output
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

What about a map??

``` r
map(weather_nest$data, weather_lm)
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

What about a map in a list column?

``` r
weather_nest = 
  weather_nest %>% 
  mutate(models = map(data, weather_lm))

weather_nest$models
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221
