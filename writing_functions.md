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

    ##  [1] -0.005606238 -0.692914308 -0.333261777 -1.006083152 -0.259697284
    ##  [6] -0.410300557  0.687486929  0.048287254 -0.089948756 -0.917124992
    ## [11] -0.110845371 -0.547030360 -0.751377423  1.653966449  1.611608088
    ## [16]  1.059724588 -1.331195370 -0.794225168 -0.255457673 -1.191100184
    ## [21]  2.332898192  0.657506438  1.320845566 -0.299749414  2.005220081
    ## [26] -0.848909337 -0.760221861  0.226676854  0.221002469 -1.220173682

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

    ##  [1]  3.0346154  0.8597877  1.9978251 -0.1311629  2.2306030  1.7540537
    ##  [7]  5.2277488  3.2051490  2.7677330  0.1503247  2.7016105  1.3214038
    ## [13]  0.6747946  8.2859502  8.1519169  6.4056090 -1.1599054  0.5392128
    ## [19]  2.2440182 -0.7166066 10.4342730  5.1328825  7.2318660  2.1038672
    ## [25]  9.3974113  0.3661774  0.6468085  3.7696217  3.7516664 -0.8086029

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
    ## 1  4.46  3.16

Check that the function works.

``` r
x_vec = rnorm(1000)
mean_and_sd(x_vec)
```

    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 0.0227 0.990

``` r
x_vec = rnorm(100, mean = 3, sd = 4)
mean_and_sd(x_vec)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.12  4.63

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
    ## 1  2.90  3.27

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
    ## 1  6.29  3.40

``` r
sim_mean_sd(samp_size = 100, mu = 6, sigma = 3)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.71  3.17

``` r
sim_mean_sd(mu = 6, samp_size = 100,  sigma = 3)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  6.27  3.18

``` r
sim_mean_sd(1000)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.72  4.01

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

    ## # A tibble: 0 x 3
    ## # … with 3 variables: title <chr>, stars <dbl>, text <chr>

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

    ## # A tibble: 10 x 3
    ##    title                         stars text                                     
    ##    <chr>                         <dbl> <chr>                                    
    ##  1 never gets old                    5 I saw this seven times in the theatres w…
    ##  2 Rex Kwon Do!                      5 If this movie was any more amazing than …
    ##  3 Good for some nostalgia           5 Holy cow, I watched this movie when it f…
    ##  4 Funny                             5 Funny wholesome movie                    
    ##  5 Voting for Pedro                  5 Arrived fast, and product as expected.   
    ##  6 Good movie                        4 It brings back memories.  My son and I w…
    ##  7 Awesome                           5 Great movie! Happy purchase for easy acc…
    ##  8 Can watch it 100 times and s…     5 One of the best.                         
    ##  9 Eat all the chips kip             5 A cult classic everyone should watch onc…
    ## 10 “Your Mom goes college”           5 “Tina come eat your dinner”

## Mean scoping example

``` r
f = function(x) {
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
my_summary = function(x, sum_func)  {
  
  sum_func(x)
}

x_vec = rnorm(100, 3, 7)

mean(x_vec)
```

    ## [1] 2.289007

``` r
median(x_vec)
```

    ## [1] 2.317581

``` r
my_summary(x_vec, IQR)
```

    ## [1] 11.72527
