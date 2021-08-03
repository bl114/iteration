Iteration
================

``` r
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.2     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
    ## ✓ readr   1.4.0     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'

    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
knitr::opts_chunk$set(
  fig.width = 6,
  fig.asp = 0.6,
  out.width = "90%")


theme_set(theme_minimal() + theme(legend.position = "bottom"))

options(
  ggplot2.continuous.colour = "viridis" ,
  ggplot2.continuous.fill = "viridis"
)

scale_colour_discrete = scale_colour_viridis_d
scale_fill_discrete = scale_fill_viridis_d
```

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -1.08229360  0.78630626 -0.75489121 -0.68064438 -1.86073529 -0.77656136
    ##  [7] -1.44353806  0.52406653  1.92190924 -0.09582191 -0.58003094  1.82157968
    ## [13]  0.95637448  0.51770621 -0.44592487  1.20336655 -0.58113203 -0.34809778
    ## [19]  1.74458505  0.58930367 -0.30552237 -0.84269116 -0.30635144 -0.04464723
    ## [25]  1.14779236  0.80479408  0.15201789 -1.63296327 -0.40274047  0.01478538

I want a function to compute z-scores

``` r
z_scores = function(x) {
  
  if (!is.numeric(x)) {
    stop("Input must be numeric")
  }
  
  if (length(x) < 3) {
    stop("Input must have at least 3 numbers")
  }
  
 z = (x - mean(x) /sd(x))
 
 return(z)
}

z_scores(x_vec)
```

    ##  [1]  0.1778685  4.9972906  1.0222924  1.2137870 -1.8298588  0.9664016
    ##  [7] -0.7538395  4.3209318  7.9261948  2.7221390  1.4732854  7.6674286
    ## [13]  5.4359241  4.3045275  1.8191667  6.0729567  1.4704455  2.0714786
    ## [19]  7.4688470  4.4891890  2.1812875  0.7958421  2.1791492  2.8541268
    ## [25]  5.9296218  5.0449737  3.3613580 -1.2423978  1.9305463  3.0074131

Try my function on some other things. These should give errors.

``` r
z_scores(3)
```

    ## Error in z_scores(3): Input must have at least 3 numbers

``` r
z_scores("my name is jeff")
```

    ## Error in z_scores("my name is jeff"): Input must be numeric

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
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("Input must be numeric")
  }
  
  if (length(x) < 3) {
    stop("Input must have at least 3 numbers")
  }
  
mean_x = mean(x)
sd_x = sd(x)

tibble(
mean = mean(x), 
sd = sd_x
)
}

mean_and_sd(x_vec)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.85  2.58

Check that the function works.

``` r
x_vec = rnorm(1000)
mean_and_sd(x_vec)
```

    ## # A tibble: 1 x 2
    ##      mean    sd
    ##     <dbl> <dbl>
    ## 1 -0.0160  1.01

``` r
x_vec = rnorm(100, mean = 3, sd = 4)
mean_and_sd(x_vec)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.13  3.92

## Multiple inputs

I’d like to do this with a function:

``` r
sim_data = 
  tibble(
    x = rnorm(n = 100, mean = 3, sd = 3)
  )

sim_data %>% 
    summarize(
    mean = mean(x),
    sd = sd(x)
  )
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.01  2.95

``` r
sim_mean_sd = function(samp_size, mu = 3, sigma = 4) {
  
  sim_data = 
  tibble(
    x = rnorm(n = samp_size, mean = mu, sd = sigma)
  )

sim_data %>% 
    summarize(
    mean = mean(x),
    sd = sd(x)
  )
  
  
}

sim_mean_sd(100, 6, 3)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  6.23  3.15

``` r
sim_mean_sd(samp_size = 100, mu = 6, sigma = 3)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  6.22  3.04

``` r
sim_mean_sd(mu = 6, samp_size = 100,  sigma = 3)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.98  3.25

``` r
sim_mean_sd(1000)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.79  3.91

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

reviews
```

    ## # A tibble: 0 x 3
    ## # … with 3 variables: title <chr>, stars <dbl>, text <chr>

What about the next page of reviews… Let’s turn that code into a
function

``` r
read_page_reviews = function(url) {
  
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
  
  reviews =
  
  tibble(
    title = review_titles,
    stars = review_stars,
    text = review_text
  )
  
  reviews
}
```

Let me try my function.

``` r
dynamite_url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

read_page_reviews(dynamite_url)
```

    ## # A tibble: 10 x 3
    ##    title                 stars text                                             
    ##    <chr>                 <dbl> <chr>                                            
    ##  1 Love it!! 💜              5 Love it!! 💜                                     
    ##  2 Funny                     5 Fiunnt                                           
    ##  3 LOVE it                   5 cult classic. So ugly it's fun. Music , acting, …
    ##  4 Perfect                   5 Exactly what I asked for                         
    ##  5 Love this movie!          5 Great movie and sent in a nice package! No damag…
    ##  6 Love it                   5 Love this movie. However the teenagers of today …
    ##  7 As described              3 Book is as described                             
    ##  8 GOSH!!!                   5 Just watch the movie GOSH!!!!                    
    ##  9 Watch it right now        5 You need to watch this movie today. It’s hilario…
    ## 10 At this point it’s a…     5 I watch this movie way too much. Have a friend o…

Let’s read a few pages of reviews.

``` r
dynamite_url_base = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber="

dynamite_urls = str_c(dynamite_url_base, 1:5)
dynamite_urls[1]
```

    ## [1] "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

``` r
all_reviews = 
bind_rows(
read_page_reviews(dynamite_urls[1]),
read_page_reviews(dynamite_urls[2]),
read_page_reviews(dynamite_urls[3]),
read_page_reviews(dynamite_urls[4]),
read_page_reviews(dynamite_urls[5])
)

all_reviews
```

    ## # A tibble: 0 x 3
    ## # … with 3 variables: title <chr>, stars <dbl>, text <chr>
