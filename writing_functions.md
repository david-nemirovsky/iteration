Writing Functions
================

## Do somethign simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)
(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -0.4576916369 -0.3285833170 -0.5061368655  0.0004275124  1.2064522794
    ##  [6] -1.7988917914 -0.2289718552  0.3271620924 -0.4842174843 -0.5106826576
    ## [11] -0.6852119831  0.1970037644 -1.7430924267  2.5590052897 -0.7125798392
    ## [16]  0.5386413990  0.7603563375 -0.1909252545  0.8613045069 -0.6316671059
    ## [21]  0.1398300298 -1.0385169215  0.2682200926  0.1449066455  1.8508043214
    ## [26]  0.7921385522 -0.0716311319 -0.8849149798 -1.1358426166  1.7633050439

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

    ##  [1] -0.4576916369 -0.3285833170 -0.5061368655  0.0004275124  1.2064522794
    ##  [6] -1.7988917914 -0.2289718552  0.3271620924 -0.4842174843 -0.5106826576
    ## [11] -0.6852119831  0.1970037644 -1.7430924267  2.5590052897 -0.7125798392
    ## [16]  0.5386413990  0.7603563375 -0.1909252545  0.8613045069 -0.6316671059
    ## [21]  0.1398300298 -1.0385169215  0.2682200926  0.1449066455  1.8508043214
    ## [26]  0.7921385522 -0.0716311319 -0.8849149798 -1.1358426166  1.7633050439

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
    ##      mean    sd
    ##     <dbl> <dbl>
    ## 1 -0.0221 0.989

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
    ## 1  4.37  2.73

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
    ## 1  5.62  2.96

## Let’s review Napoleon Dynamite

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

dynamite_html = read_html(url)

review_titles = 
  dynamite_html %>%
  html_nodes(".a-text-bold span") %>%
  html_text()

review_stars = 
  dynamite_html %>%
  html_nodes("#cm_cr-review_list .review-rating") %>%
  html_text() %>%
  str_extract("^\\d") %>%
  as.numeric()

review_text = 
  dynamite_html %>%
  html_nodes(".review-text-content span") %>%
  html_text() %>% 
  str_replace_all("\n", "") %>% 
  str_trim()

reviews = tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)
```

What about the next page of reviews??

Let’s turn that code into a function:

``` r
read_page_reviews = function(url){
  
  html = read_html(url)
  
  review_titles = 
    html %>%
    html_nodes(".a-text-bold span") %>%
    html_text()
  
  review_stars = 
    html %>%
    html_nodes("#cm_cr-review_list .review-rating") %>%
    html_text() %>%
    str_extract("^\\d") %>%
    as.numeric()
  
  review_text = 
    html %>%
    html_nodes(".review-text-content span") %>%
    html_text() %>% 
    str_replace_all("\n", "") %>% 
    str_trim()
  
  reviews = tibble(
    title = review_titles,
    stars = review_stars,
    text = review_text
  )
  
  reviews
  
}
```

Let me try my function:

``` r
dynamite_url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=5"
read_page_reviews(dynamite_url)
```

    ## # A tibble: 10 x 3
    ##    title                stars text                                              
    ##    <chr>                <dbl> <chr>                                             
    ##  1 Vote for Pedro!!!        5 Simply the greatest low budget film of all time!!~
    ##  2 Good movie for ever~     5 This movie is a family favorite.                  
    ##  3 Dynamite                 5 Great price & quality                             
    ##  4 Classic                  5 Classic                                           
    ##  5 Rip off                  1 Do not buy it was a rip off                       
    ##  6 Hilarious Movie!         5 This is the best movie! I can never get sick of w~
    ##  7 A must watch.            5 A very unique and fun film for pretty much everyo~
    ##  8 Favorite movie           5 Dry comedy I've loved since I was a kid. Vote for~
    ##  9 silly.. always make~     5 Freaking hilarious. It is one of those movies tha~
    ## 10 Sweet                    5 Sweet

Let’s read a few pages of reviews:

``` r
dynamite_url_base = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber="

dynamite_urls = str_c(dynamite_url_base, 1:5)

all_reviews = 
  bind_rows(
    read_page_reviews(dynamite_urls[1]),
    read_page_reviews(dynamite_urls[2]),
    read_page_reviews(dynamite_urls[3]),
    read_page_reviews(dynamite_urls[4]),
    read_page_reviews(dynamite_urls[5])
)
```
