lis_col
================
Sarahy Martinez
2024-10-29

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'
    ## 
    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
knitr::opts_chunk$set(
  fig.width = 6,
  fig.asp = .6,
  out.width = "90%:"
  
)


theme_set(theme_minimal()+ theme(legend.position = "bottom"))


options(
  ggplot2.continuous.colour = "viridis",
  ggplot2.continuous.fill = "viridis"
)
```

## Lists

You can put anything in a list

``` r
l = list (
 # no way we can put all of this information into a dataframe so we will make a list
vec_numeric = 5:8,
vec_logical = c(TRUE, TRUE , FALSE, TRUE, FALSE, FALSE),
mat = matrix(1:8, nrow = 2, ncol= 4),
summary = summary(rnorm(100)) 
  
)
```

``` r
l  # now we have a way of storing everyhting instead of different datafra,s 
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
    ## -3.0403 -0.8844 -0.1804 -0.1865  0.4774  2.3631

``` r
l$vec_numeric #will give you data of just vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[1]] # returns first list
```

    ## [1] 5 6 7 8

``` r
l[["vec_numeric"]]  #also returns first list 
```

    ## [1] 5 6 7 8

``` r
mean(l[["vec_numeric"]]) # if accessing individual elements of the list
```

    ## [1] 6.5

## `for` loop

Create a new list.

``` r
list_norm = 
    list(
      a = rnorm(20, mean = 3, sd= 1),
      b = rnorm(30, mean = 0, sd= 5),
      c = rnorm(40, mean = 10, sd= .2),
      d = rnorm(20, mean = -3, sd= 1)
    )

#basis is that this is something that we cannot easily do individually 
```

``` r
list_norm[[1]] # first element of your list 
```

    ##  [1] 2.612374 2.343652 2.517396 3.483215 1.920034 2.768356 3.678215 4.584509
    ##  [9] 3.703811 3.685624 2.884562 2.485515 3.646138 2.946642 1.424131 1.604043
    ## [17] 3.279572 3.793146 2.520898 4.031364

pause anf get old function

- write a function that give the mean and standard deviation

``` r
mean_and_sd = function(x) {
  if (!is.numeric(x)) {
    stop("Argument x should be numeric")
  } 
  
  if (length(x) <3) {
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

I can apply this function to each list element

``` r
mean_and_sd(list_norm[[1]]) # don't forget to load libraries! , can get mean and std deviation of the lists
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.00 0.838

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.27  5.46

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.214

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.34 0.968

``` r
# even though we have a function we have to copy them 1-4 times, can be annoying if  we had 4 so we will go into for list loop
```

Let’s use a for a loop

``` r
output = vector("list", length = 4)

for( i in 1:4){

output[[i]] = mean_and_sd(list_norm[[i]]) # possible but we're gonna have to do this 40 times and that's no fun

}


# keep track of your i and see where you have to change and replace 
```

## Let’s try map!

if we can take map as a function and give input list with function we
want to apply hopefull it will do the same process

``` r
map(list_norm, mean_and_sd)   #gives you output same as the loop
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.00 0.838
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.27  5.46
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.214
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.34 0.968

``` r
output = map(list_norm, mean_and_sd) # these two code chunks will be equivalent with the difference that map keeps track of input names
```

what if you want a different function?

``` r
output = map(list_norm, median) # gives you the median instead of mean and sd 

output = map(list_norm, IQR)  # you can map any functions you want across the input list, can apply functions to any element of your input list and the map statement should be clear
```
