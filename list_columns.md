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
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -2.54490 -0.74828 -0.19452 -0.03675  0.78328  2.60060

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

    ##  [1] 3.3684005 3.1312858 2.5748232 2.5059093 2.4998184 4.3962263 2.7031699
    ##  [8] 2.3927776 3.7242341 2.7857286 1.8661845 2.2202539 3.7902699 2.0170068
    ## [15] 0.5806744 2.3143434 3.1190048 3.2498248 3.3326155 3.5742653

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
    ## 1  2.81 0.839

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.514  5.24

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.99 0.221

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.20  1.05

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
    ## 1  2.81 0.839
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.514  5.24
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.99 0.221
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.20  1.05

``` r
output = map(list_norm, mean_and_sd) # these two code chunks will be equivalent with the difference that map keeps track of input names
```

what if you want a different function?

``` r
output = map(list_norm, median) # gives you the median instead of mean and sd 

output = map(list_norm, IQR)  # you can map any functions you want across the input list, can apply functions to any element of your input list and the map statement should be clear
```

we know when we run input we get A,B,D,D and its a list. So what if its
a dbl?

``` r
output = map_dbl(list_norm, median)


output = map_dbl(list_norm, median, .id = "input")
```

``` r
output = map_df(list_norm, mean_and_sd, .id = "input")  # we cant call it dbl bc they're not individual numbers, lets try to make it a df
                                            # will take each df and collapse, keeping track of id a, b,c,d ( list names) will be helpful. tells put the list names into an input column.
```

We’ve got for loops, map statements, lists, we are keeping track of
everything. we dont want all of this stuff where output came from this
list etc. So we are going into list columns.

Create a df that has a list in it.

## List Columns!

``` r
listcol_df = 
  tibble(
    name = c("a", "b", "c", "d"),
    samp = list_norm
  )

# sure theres a lot of stuff but the printed table is making easier by saying you have a list in this column with a collection of dbls 
```

``` r
listcol_df %>%  pull(name)
```

    ## [1] "a" "b" "c" "d"

``` r
listcol_df %>%  pull(samp)
```

    ## $a
    ##  [1] 3.3684005 3.1312858 2.5748232 2.5059093 2.4998184 4.3962263 2.7031699
    ##  [8] 2.3927776 3.7242341 2.7857286 1.8661845 2.2202539 3.7902699 2.0170068
    ## [15] 0.5806744 2.3143434 3.1190048 3.2498248 3.3326155 3.5742653
    ## 
    ## $b
    ##  [1]  -3.3238749  -2.8294310  -0.2625369  -4.7567486   3.0898855  -4.0804131
    ##  [7]   4.8647865   6.9611818   1.5611377   1.2241030   3.1474210  -8.8633590
    ## [13]  -0.7196114   1.3594327   5.3380236  10.8094280  -7.7385748 -11.4999752
    ## [19]   8.9991054  -2.5126642   2.5610556   2.6027883   4.2563613   3.1194810
    ## [25]   3.1177253   1.9719924   5.1971678  -7.8659053   0.3208477  -0.6352432
    ## 
    ## $c
    ##  [1] 10.013937 10.179722 10.130590  9.822910 10.242683 10.272207 10.215787
    ##  [8] 10.075182  9.828141 10.144090  9.854954  9.798424  9.949014 10.354306
    ## [15]  9.844521  9.728978 10.181375 10.055413 10.468013 10.074395  9.777548
    ## [22]  9.650785  9.775491  9.908727 10.173963  9.987945  9.949312 10.395315
    ## [29] 10.024641  9.766850  9.775663  9.808919 10.120641  9.725458 10.053738
    ## [36] 10.065423  9.624701 10.001949 10.284329  9.604747
    ## 
    ## $d
    ##  [1] -4.745905 -4.251628 -3.519512 -2.649190 -3.297683 -2.573798 -2.921643
    ##  [8] -1.964118 -1.916948 -4.193746 -2.370555 -1.518417 -3.905393 -4.037547
    ## [15] -5.003222 -4.459250 -1.851983 -2.748674 -2.473110 -3.561283

``` r
# its a df and we can sample 


listcol_df %>%  filter(name == "a")
```

    ## # A tibble: 1 × 2
    ##   name  samp        
    ##   <chr> <named list>
    ## 1 a     <dbl [20]>

Lets try some operations

``` r
listcol_df$samp  # dollar is same as pulling and gives you a list 
```

    ## $a
    ##  [1] 3.3684005 3.1312858 2.5748232 2.5059093 2.4998184 4.3962263 2.7031699
    ##  [8] 2.3927776 3.7242341 2.7857286 1.8661845 2.2202539 3.7902699 2.0170068
    ## [15] 0.5806744 2.3143434 3.1190048 3.2498248 3.3326155 3.5742653
    ## 
    ## $b
    ##  [1]  -3.3238749  -2.8294310  -0.2625369  -4.7567486   3.0898855  -4.0804131
    ##  [7]   4.8647865   6.9611818   1.5611377   1.2241030   3.1474210  -8.8633590
    ## [13]  -0.7196114   1.3594327   5.3380236  10.8094280  -7.7385748 -11.4999752
    ## [19]   8.9991054  -2.5126642   2.5610556   2.6027883   4.2563613   3.1194810
    ## [25]   3.1177253   1.9719924   5.1971678  -7.8659053   0.3208477  -0.6352432
    ## 
    ## $c
    ##  [1] 10.013937 10.179722 10.130590  9.822910 10.242683 10.272207 10.215787
    ##  [8] 10.075182  9.828141 10.144090  9.854954  9.798424  9.949014 10.354306
    ## [15]  9.844521  9.728978 10.181375 10.055413 10.468013 10.074395  9.777548
    ## [22]  9.650785  9.775491  9.908727 10.173963  9.987945  9.949312 10.395315
    ## [29] 10.024641  9.766850  9.775663  9.808919 10.120641  9.725458 10.053738
    ## [36] 10.065423  9.624701 10.001949 10.284329  9.604747
    ## 
    ## $d
    ##  [1] -4.745905 -4.251628 -3.519512 -2.649190 -3.297683 -2.573798 -2.921643
    ##  [8] -1.964118 -1.916948 -4.193746 -2.370555 -1.518417 -3.905393 -4.037547
    ## [15] -5.003222 -4.459250 -1.851983 -2.748674 -2.473110 -3.561283

``` r
listcol_df$samp[[1]] 
```

    ##  [1] 3.3684005 3.1312858 2.5748232 2.5059093 2.4998184 4.3962263 2.7031699
    ##  [8] 2.3927776 3.7242341 2.7857286 1.8661845 2.2202539 3.7902699 2.0170068
    ## [15] 0.5806744 2.3143434 3.1190048 3.2498248 3.3326155 3.5742653

``` r
# we want mean and sd 


mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.81 0.839

What if we want to apply it all the way through. Can’t we just map?
