Writing Functions
================

## Do somethign simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)
(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -0.4193450 -2.6559228  0.6208432  2.1953816  0.4367711 -0.9685947
    ##  [7] -0.4741085 -0.5127056 -1.0105582  1.3818421  0.4100363  1.5520775
    ## [13] -1.1862592 -0.3924192  0.2627205  0.6144119 -0.2926148 -0.3074601
    ## [19] -0.2796483 -0.2278744 -1.7091162  0.1115115  0.9748685 -0.3975385
    ## [25]  1.0918338  0.1040549  0.7024292 -0.5348304 -0.1297619  1.0399755

I want a function to compute z-scores:

``` r
z_scores = function(x){
  
  if(!is.numeric(x)) {
    stop("Input must be numeric")
  }
  
  if(length(x) < 3) {
    stop("Input must have at least 3 numbers")
  }
  
  z = (x - mean(x)) / sd(x)
  
  return(z)
  
}

z_scores(x_vec)
```

    ##  [1] -0.4193450 -2.6559228  0.6208432  2.1953816  0.4367711 -0.9685947
    ##  [7] -0.4741085 -0.5127056 -1.0105582  1.3818421  0.4100363  1.5520775
    ## [13] -1.1862592 -0.3924192  0.2627205  0.6144119 -0.2926148 -0.3074601
    ## [19] -0.2796483 -0.2278744 -1.7091162  0.1115115  0.9748685 -0.3975385
    ## [25]  1.0918338  0.1040549  0.7024292 -0.5348304 -0.1297619  1.0399755

Try my function on some other things that should give errors:

``` r
z_scores(3)
```

    ## Error in z_scores(3): Input must have at least 3 numbers

``` r
z_scores("my name is david")
```

    ## Error in z_scores("my name is david"): Input must be numeric

``` r
z_scores(mtcars)
```

    ## Error in z_scores(mtcars): Input must be numeric

``` r
z_scores(c(TRUE, TRUE, FALSE, TRUE))
```

    ## Error in z_scores(c(TRUE, TRUE, FALSE, TRUE)): Input must be numeric

## Multiple outputs

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

Check that the fct works:

``` r
x_vec = rnorm(1000)
mean_and_sd(x_vec)
```

    ## # A tibble: 1 x 2
    ##       mean    sd
    ##      <dbl> <dbl>
    ## 1 0.000783  1.04

## Multiple inputs

I’d like to do this with a function:

``` r
sim_data = 
  tibble(
    x = rnorm(n = 100, mean = 4, sd = 3)
  )

sim_data %>% 
  summarize(mean = mean(x), sd = sd(x))
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.16  3.00

Here’s the function:

``` r
sim_mean_sd = function(n, mu, sigma) {
  
  sim_data = 
    tibble(
      x = rnorm(n = n, mean = mu, sd = sigma)
      )

sim_data %>% 
  summarize(mean = mean(x), sd = sd(x))
  
}

sim_mean_sd(100, 6, 3)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  6.17  3.12
