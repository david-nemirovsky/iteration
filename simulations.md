Simulations
================

## Let’s simulate something

I have a function:

``` r
sim_mean_sd = function(n, mu = 3, sigma = 4) {
  
  sim_data = 
    tibble(
      x = rnorm(n = n, mean = mu, sd = sigma)
      )

sim_data %>% 
  summarize(mean = mean(x), sd = sd(x))
  
}
```

I can simulate something:

``` r
sim_mean_sd(30)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.18  2.93
