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
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -2.63000 -0.77923 -0.06757 -0.09130  0.51942  2.53262

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
    ##  [1] 2.9575188 4.2180334 2.5993228 5.5760986 2.8555250 3.2778270 4.0146067
    ##  [8] 4.3626479 3.3501255 2.3551689 3.3621430 1.7018781 4.4938264 1.3662842
    ## [15] 1.8685218 3.3951260 3.5262205 1.1991372 0.7388148 4.3095199
    ## 
    ## $b
    ##  [1]   8.2545926  -2.6918546  12.4094967  -4.8500547   4.9708031  -3.4182816
    ##  [7] -12.9129909  -0.3593815   3.7518235   3.0977277  -0.3205480   3.2991153
    ## [13]   3.0447687   1.2062768   4.9475980  10.8184465   0.3157980  -0.5440534
    ## [19]   1.6437303  -1.1469791
    ## 
    ## $c
    ##  [1]  9.874643  9.730808  9.775834 10.183748  9.735162  9.664683  9.879891
    ##  [8]  9.952575  9.807804 10.022139 10.173834  9.904878 10.207777 10.129531
    ## [15]  9.995802  9.853145 10.198005  9.763037 10.113106  9.880031
    ## 
    ## $d
    ##  [1] -2.8280366 -2.5955471 -3.0036057 -3.7777037 -2.2159537 -2.4795293
    ##  [7] -3.0155653 -3.8624655 -4.7644926 -4.6567139 -3.7783131 -3.2271542
    ## [13] -1.6632888 -2.4869144 -0.6367536 -3.0816152 -2.9165912 -1.8853159
    ## [19] -3.8540208 -4.0958692

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
    ## 1  3.08  1.26

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.58  5.61

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.94 0.176

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.04  1.02

Let’s use a `for` loop:

``` r
output = vector("list", length = 4)

for(i in 1:4) {

  output[[i]] = mean_and_sd(list_norm[[i]])

}
```
