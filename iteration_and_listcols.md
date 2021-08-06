Iteration and Listcols
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
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ## -3.6093 -0.5992  0.2001  0.1096  0.8456  2.4921

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

## ‘for’ loop

Create a new list.

``` r
list_norm =
  list(
    a = rnorm(20, mean = 3, sd = 1), 
    b = rnorm(20, mean = 0, sd = 5),
    c = rnorm(40, mean = 10, sd = 2),
    d = rnorm(20, mean = -3, sd = 1)
  )
```

``` r
list_norm[[1]]
```

    ##  [1] 1.489877 2.790260 3.610163 3.988585 3.235571 4.418693 3.353819 3.207379
    ##  [9] 2.781720 2.190106 2.195614 1.936827 2.993546 2.814199 1.764283 2.790055
    ## [17] 3.873254 2.840070 2.488263 3.849913

Pause and get my old function

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
```

I can apply that function to each list element

``` r
mean_and_sd(list_norm[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.93 0.784

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.802  4.74

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.59  2.17

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.47  1.22

Let’s use a ‘for’ loop

``` r
output = vector("list", length = 4)
output[[1]] = mean_and_sd(list_norm[[1]])

for (i in 1:4) {
  
  output[[i]] = mean_and_sd(list_norm[[i]])
  
}
```

## Let’s try map!

``` r
output =
  
map(list_norm, mean_and_sd)
```

What if you want a different function?

``` r
map(list_norm, median)
```

    ## $a
    ## [1] 2.827134
    ## 
    ## $b
    ## [1] 0.7529702
    ## 
    ## $c
    ## [1] 9.493187
    ## 
    ## $d
    ## [1] -2.240738

``` r
map(list_norm, IQR)
```

    ## $a
    ## [1] 1.002805
    ## 
    ## $b
    ## [1] 6.525693
    ## 
    ## $c
    ## [1] 3.454225
    ## 
    ## $d
    ## [1] 1.612357

``` r
output =
  
map_dbl(list_norm, median)
```

``` r
output =
  
map_df(list_norm, mean_and_sd, .id = "input")
```

## List columns!

``` r
listcol_df =
  tibble(
    name = c("a", "b", "c", "d"),
    samp = list_norm
  )
```

``` r
listcol_df %>% pull(name)
```

    ## [1] "a" "b" "c" "d"

``` r
listcol_df %>% pull(samp)
```

    ## $a
    ##  [1] 1.489877 2.790260 3.610163 3.988585 3.235571 4.418693 3.353819 3.207379
    ##  [9] 2.781720 2.190106 2.195614 1.936827 2.993546 2.814199 1.764283 2.790055
    ## [17] 3.873254 2.840070 2.488263 3.849913
    ## 
    ## $b
    ##  [1]  2.71178754 -2.55898642 -4.41027632  4.83077778  4.05178564 -0.93610924
    ##  [7] -0.68964727  1.36832101 -3.10271319  1.41386216 -2.38023036 -5.29964766
    ## [13]  4.24773584 -0.03458611 -8.16276622  4.27181653  5.79206866  1.07094947
    ## [19]  0.43499096 13.42356574
    ## 
    ## $c
    ##  [1]  7.973810 10.523256  8.771200 12.136194  8.413500 13.258218  7.223263
    ##  [8]  9.522425  9.451867 12.137176  8.095441  8.532085 11.018315  5.518479
    ## [15]  9.463949  8.057467  6.870201  8.450638  6.702101 12.644327  9.545584
    ## [22]  7.759188 13.104596 10.987492 12.257373 11.889498 11.480288  5.710282
    ## [29]  6.137200 11.731439  8.959722  7.593615 10.366048 11.522249  9.135127
    ## [36]  6.540088 10.419558 11.043939 10.451683 12.377488
    ## 
    ## $d
    ##  [1]  0.2452147 -2.2031916 -2.2782836 -4.0529117 -2.9208002 -1.9848191
    ##  [7] -1.8742072 -1.8164526 -0.8763084 -3.4486395 -3.5877266 -3.7439166
    ## [13] -0.9906308 -3.5373487 -1.8724616 -3.1030444 -3.0751449 -4.7554158
    ## [19] -1.9302021 -1.6289078

``` r
listcol_df %>% 
  filter(name == "a")
```

    ## # A tibble: 1 x 2
    ##   name  samp        
    ##   <chr> <named list>
    ## 1 a     <dbl [20]>

Let’s try some operations

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.93 0.784

``` r
mean_and_sd(listcol_df$samp[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.802  4.74

Can I just map?

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.93 0.784
    ## 
    ## $b
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.802  4.74
    ## 
    ## $c
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.59  2.17
    ## 
    ## $d
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.47  1.22

So…can I add a list column??

``` r
listcol_df =
listcol_df %>% 
  mutate(
    summary = map(samp, mean_and_sd),
    medians = map_dbl(samp, median)
  )
```
