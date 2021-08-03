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

    ##  [1]  0.60826571  1.18787023  0.83787955  0.59602143 -2.64221402 -0.56187852
    ##  [7]  1.34044603 -1.91373230  0.14525435  1.41927839  0.96528595  0.96561627
    ## [13] -0.32860408  1.57593560 -0.81158070  0.07185362  0.11851609  0.58483702
    ## [19] -1.25043051  0.31424491 -1.34740692 -0.32118581 -0.68831182 -0.50187898
    ## [25] -0.71420852 -0.15322802  0.31834923 -0.45010327  0.84128198 -0.20617289

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

    ##  [1]  4.6027608  6.1738285  5.2251488  4.5695716 -4.2079434  1.4309844
    ##  [7]  6.5873983 -2.2333312  3.3477287  6.8010802  5.5704947  5.5713901
    ## [13]  2.0632948  7.2257130  0.7541452  3.1487697  3.2752524  4.5392553
    ## [19] -0.4353948  3.8057921 -0.6982577  2.0834027  1.0882761  1.5936183
    ## [25]  1.0180809  2.5386667  3.8169173  1.7339608  5.2343713  2.3951550

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
