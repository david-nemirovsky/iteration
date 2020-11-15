Writing Functions
================

## Do somethign simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)
(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -0.08645268 -1.16141274 -1.66155875 -0.33105315 -1.03405305  0.30347981
    ##  [7]  1.32011655 -0.49623619  0.26983152 -0.54450824 -0.97283131 -0.15337533
    ## [13]  2.04938586  1.07217493  1.01744837  1.15002435 -1.96128580  1.70389867
    ## [19] -1.67852616 -0.60979416 -0.75228333  0.59028416 -0.02114382  0.35310308
    ## [25]  0.22295791 -0.42266582  0.45715508  0.42730701 -0.03808206  0.98809527

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

    ##  [1] -0.08645268 -1.16141274 -1.66155875 -0.33105315 -1.03405305  0.30347981
    ##  [7]  1.32011655 -0.49623619  0.26983152 -0.54450824 -0.97283131 -0.15337533
    ## [13]  2.04938586  1.07217493  1.01744837  1.15002435 -1.96128580  1.70389867
    ## [19] -1.67852616 -0.60979416 -0.75228333  0.59028416 -0.02114382  0.35310308
    ## [25]  0.22295791 -0.42266582  0.45715508  0.42730701 -0.03808206  0.98809527

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
