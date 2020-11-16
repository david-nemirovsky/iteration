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
    ##      Min.   1st Qu.    Median      Mean   3rd Qu.      Max. 
    ## -2.326837 -0.698205  0.002502  0.041678  0.668684  2.792626

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
    ##  [1] 2.741883 1.230314 1.891666 1.709956 3.737586 4.321760 2.284866 1.482138
    ##  [9] 4.455541 3.603392 3.137233 3.335337 1.149696 1.881650 2.795391 2.938460
    ## [17] 2.783855 2.966368 4.485960 3.682368
    ## 
    ## $b
    ##  [1]  0.8763223  3.6222217  0.7373036 -0.5379671  0.9955015 -0.2961362
    ##  [7] -9.5316370  0.4623646  8.2010860 -2.2376850 -0.2962147  8.1794284
    ## [13] -7.9894681 -0.5240826  6.5328318 -1.5265992  2.6571183  1.0944167
    ## [19]  0.1815031  2.6631887
    ## 
    ## $c
    ##  [1]  9.985116 10.063078  9.764836  9.983669 10.005102 10.105917  9.862814
    ##  [8]  9.651611  9.999831  9.834716 10.238193 10.068136  9.731968  9.828344
    ## [15] 10.287109  9.880657 10.189235 10.151720  9.974909 10.047994
    ## 
    ## $d
    ##  [1] -0.8519431 -3.0375898 -2.1987182 -1.9029657 -3.6761158 -2.8069064
    ##  [7] -2.6171804 -4.7055722 -3.3519723 -1.9435337 -4.0368409 -3.7720124
    ## [13] -2.3121389 -3.8642242 -2.1421694 -3.3590727 -2.7974397 -3.0224690
    ## [19] -3.7155346 -3.1409045

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
    ## 1  2.83  1.04

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.663  4.36

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.98 0.171

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.96 0.905

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
    ##  [1] 2.741883 1.230314 1.891666 1.709956 3.737586 4.321760 2.284866 1.482138
    ##  [9] 4.455541 3.603392 3.137233 3.335337 1.149696 1.881650 2.795391 2.938460
    ## [17] 2.783855 2.966368 4.485960 3.682368
    ## 
    ## $b
    ##  [1]  0.8763223  3.6222217  0.7373036 -0.5379671  0.9955015 -0.2961362
    ##  [7] -9.5316370  0.4623646  8.2010860 -2.2376850 -0.2962147  8.1794284
    ## [13] -7.9894681 -0.5240826  6.5328318 -1.5265992  2.6571183  1.0944167
    ## [19]  0.1815031  2.6631887
    ## 
    ## $c
    ##  [1]  9.985116 10.063078  9.764836  9.983669 10.005102 10.105917  9.862814
    ##  [8]  9.651611  9.999831  9.834716 10.238193 10.068136  9.731968  9.828344
    ## [15] 10.287109  9.880657 10.189235 10.151720  9.974909 10.047994
    ## 
    ## $d
    ##  [1] -0.8519431 -3.0375898 -2.1987182 -1.9029657 -3.6761158 -2.8069064
    ##  [7] -2.6171804 -4.7055722 -3.3519723 -1.9435337 -4.0368409 -3.7720124
    ## [13] -2.3121389 -3.8642242 -2.1421694 -3.3590727 -2.7974397 -3.0224690
    ## [19] -3.7155346 -3.1409045

Let’s try some operations:

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.83  1.04

``` r
mean_and_sd(listcol_df$samp[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.663  4.36

Can I just map??

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.83  1.04
    ## 
    ## $b
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.663  4.36
    ## 
    ## $c
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.98 0.171
    ## 
    ## $d
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.96 0.905

So, can I add a list column??

``` r
listcol_df = 
  listcol_df %>% 
    mutate(summary = map(samp, mean_and_sd),
           medians = map_dbl(samp, median))
```
