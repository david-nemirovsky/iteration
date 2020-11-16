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
    ## -2.30375 -0.70601  0.01807 -0.06286  0.56793  2.71880

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
    ##  [1] 2.805333 2.374145 2.187323 3.174322 3.271225 3.329957 3.192432 3.984356
    ##  [9] 2.014593 2.364047 2.614842 1.171388 3.192352 2.843521 4.080101 3.146147
    ## [17] 3.145792 3.266819 4.184852 3.780473
    ## 
    ## $b
    ##  [1] -2.4972277  6.0576807  0.4498451 -3.3237561 -0.9090542 -0.5607807
    ##  [7]  9.4476069  0.5456748  2.3572060 -2.3160589 -3.1840256  0.4262305
    ## [13]  1.0031865 -3.1394624 -7.5706790  0.7644244  3.0822561 -8.9419328
    ## [19] -2.6481811  1.5528895
    ## 
    ## $c
    ##  [1] 10.190030 10.034852  9.769601  9.770206 10.151822  9.706038  9.589903
    ##  [8]  9.903293  9.674603  9.977444 10.187116 10.056109  9.980097 10.014946
    ## [15] 10.188045 10.061227  9.828455 10.113811  9.882360  9.841431
    ## 
    ## $d
    ##  [1] -1.220070 -3.247815 -1.965662 -3.512901 -2.271602 -4.787187 -2.503718
    ##  [8] -1.158756 -3.383950 -4.052028 -1.955799 -3.016252 -2.645380 -3.602168
    ## [15] -2.646228 -3.825809 -2.697759 -4.440916 -3.129105 -2.896550

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
    ## 1  3.01 0.744

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 -0.470  4.16

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.95 0.183

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.95 0.965

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

## List Columns
