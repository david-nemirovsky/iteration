Writing Functions
================

## Do somethign simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)
(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -1.77487819  0.21844034  1.08743826 -0.58522783 -0.04871531  0.68109226
    ##  [7]  1.88977889  1.47581007 -0.03413857 -0.48565423 -0.13290909  1.98985795
    ## [13]  1.43767345 -1.20728274 -0.47428228 -0.63903510 -0.26752645 -0.27021238
    ## [19]  1.25768745 -0.85211931  0.59506289  0.63434236 -0.75897971  0.35952150
    ## [25] -0.92894242 -0.35603835 -1.01180594 -1.13973771 -1.38922421  0.73000440

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

    ##  [1] -1.77487819  0.21844034  1.08743826 -0.58522783 -0.04871531  0.68109226
    ##  [7]  1.88977889  1.47581007 -0.03413857 -0.48565423 -0.13290909  1.98985795
    ## [13]  1.43767345 -1.20728274 -0.47428228 -0.63903510 -0.26752645 -0.27021238
    ## [19]  1.25768745 -0.85211931  0.59506289  0.63434236 -0.75897971  0.35952150
    ## [25] -0.92894242 -0.35603835 -1.01180594 -1.13973771 -1.38922421  0.73000440

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
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 0.0200  1.01

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
    ## 1  4.69  3.00

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
    ## 1  5.87  3.29

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

## Mean scoping example

``` r
f = function(x){
  
  z = x + y
  
  z
  
}

x = 1
y = 2

f(x = y)
```

    ## [1] 4

## Functions as arguments

``` r
my_summary = function(x, summ_function){
  
  summ_function(x)
  
}

x_vec = rnorm(100, 3, 7)

mean(x_vec)
```

    ## [1] 2.479414

``` r
median(x_vec)
```

    ## [1] 2.881572

``` r
my_summary(x_vec, IQR)
```

    ## [1] 10.21535
