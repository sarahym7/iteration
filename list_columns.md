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
    ## -2.46618 -0.56740 -0.09090 -0.09907  0.49939  2.53734

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

    ##  [1] 4.4174929 2.9108904 2.9113857 2.2871243 3.7920771 3.1440684 2.5346210
    ##  [8] 1.9942922 3.7841590 1.2618689 3.8643738 3.6705524 1.4241531 3.2812332
    ## [15] 1.7425285 4.6071002 1.9805377 4.1269145 0.7746952 2.6844229

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
    ## 1  2.86  1.10

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.10  4.47

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.96 0.191

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.83 0.662

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
    ## 1  2.86  1.10
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.10  4.47
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.96 0.191
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.83 0.662

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
    ##  [1] 4.4174929 2.9108904 2.9113857 2.2871243 3.7920771 3.1440684 2.5346210
    ##  [8] 1.9942922 3.7841590 1.2618689 3.8643738 3.6705524 1.4241531 3.2812332
    ## [15] 1.7425285 4.6071002 1.9805377 4.1269145 0.7746952 2.6844229
    ## 
    ## $b
    ##  [1]  2.37591931 -1.93377668  1.69636371  5.86869410  5.07118690  9.70307340
    ##  [7]  1.16077589  4.14056420 -0.90208278 -5.20508796  0.68683789  4.57792255
    ## [13] -5.40907859  7.57411664 -1.96571841 -5.87466518  2.53487909 -2.25876575
    ## [19] -4.68270322 -1.23499249  4.57067367  2.58789112 -6.76237429 -4.66008370
    ## [25]  2.18029371  6.22123332  4.68162644  7.74991413  0.40050853  0.09176654
    ## 
    ## $c
    ##  [1] 10.027850  9.978864 10.077542  9.985662  9.905040 10.124544 10.237189
    ##  [8]  9.957601 10.198491  9.880179 10.064523  9.977055  9.861077 10.220107
    ## [15]  9.573597  9.822042  9.958583 10.032853  9.964034 10.144353 10.074068
    ## [22]  9.506099  9.711393 10.004055 10.349072  9.566835  9.936163 10.120097
    ## [29]  9.937595  9.790609 10.019135 10.122414  9.657043  9.963068 10.101516
    ## [36]  9.660447  9.997086  9.989404  9.861734 10.189652
    ## 
    ## $d
    ##  [1] -3.214248 -2.157609 -2.793287 -3.301190 -3.115226 -2.269234 -2.049815
    ##  [8] -2.802236 -2.832398 -3.737078 -3.263707 -1.352686 -1.995436 -3.016571
    ## [15] -2.243490 -2.929114 -3.224556 -4.100820 -2.764416 -3.500205

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
    ##  [1] 4.4174929 2.9108904 2.9113857 2.2871243 3.7920771 3.1440684 2.5346210
    ##  [8] 1.9942922 3.7841590 1.2618689 3.8643738 3.6705524 1.4241531 3.2812332
    ## [15] 1.7425285 4.6071002 1.9805377 4.1269145 0.7746952 2.6844229
    ## 
    ## $b
    ##  [1]  2.37591931 -1.93377668  1.69636371  5.86869410  5.07118690  9.70307340
    ##  [7]  1.16077589  4.14056420 -0.90208278 -5.20508796  0.68683789  4.57792255
    ## [13] -5.40907859  7.57411664 -1.96571841 -5.87466518  2.53487909 -2.25876575
    ## [19] -4.68270322 -1.23499249  4.57067367  2.58789112 -6.76237429 -4.66008370
    ## [25]  2.18029371  6.22123332  4.68162644  7.74991413  0.40050853  0.09176654
    ## 
    ## $c
    ##  [1] 10.027850  9.978864 10.077542  9.985662  9.905040 10.124544 10.237189
    ##  [8]  9.957601 10.198491  9.880179 10.064523  9.977055  9.861077 10.220107
    ## [15]  9.573597  9.822042  9.958583 10.032853  9.964034 10.144353 10.074068
    ## [22]  9.506099  9.711393 10.004055 10.349072  9.566835  9.936163 10.120097
    ## [29]  9.937595  9.790609 10.019135 10.122414  9.657043  9.963068 10.101516
    ## [36]  9.660447  9.997086  9.989404  9.861734 10.189652
    ## 
    ## $d
    ##  [1] -3.214248 -2.157609 -2.793287 -3.301190 -3.115226 -2.269234 -2.049815
    ##  [8] -2.802236 -2.832398 -3.737078 -3.263707 -1.352686 -1.995436 -3.016571
    ## [15] -2.243490 -2.929114 -3.224556 -4.100820 -2.764416 -3.500205

``` r
listcol_df$samp[[1]] 
```

    ##  [1] 4.4174929 2.9108904 2.9113857 2.2871243 3.7920771 3.1440684 2.5346210
    ##  [8] 1.9942922 3.7841590 1.2618689 3.8643738 3.6705524 1.4241531 3.2812332
    ## [15] 1.7425285 4.6071002 1.9805377 4.1269145 0.7746952 2.6844229

``` r
# we want mean and sd 


mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.86  1.10

What if we want to apply it all the way through. Can’t we just map?

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.86  1.10
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.10  4.47
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.96 0.191
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.83 0.662

``` r
# we are mapping a list col and applying the mean and sd to the listcol on the samp
# Suppose we want to keep track of the output and its stored outside of df. We want to appy the map to the list col and save it in a df
```

Can I add a list col? Yes

``` r
listcol_df %>% 
  mutate(summary = map(samp, mean_and_sd)) #to map across a column and apply the same function above, map across a colulmn in this dataset and we want ot map mean and STD. 
```

    ## # A tibble: 4 × 3
    ##   name  samp         summary         
    ##   <chr> <named list> <named list>    
    ## 1 a     <dbl [20]>   <tibble [1 × 2]>
    ## 2 b     <dbl [30]>   <tibble [1 × 2]>
    ## 3 c     <dbl [40]>   <tibble [1 × 2]>
    ## 4 d     <dbl [20]>   <tibble [1 × 2]>

``` r
#tibble was created! now lets do a list col df and overwrite that 


listcol_df = 
  listcol_df %>% 
  mutate(summary = map(samp, mean_and_sd),
         medians= map_dbl(samp, median))

# we're not losing information and have vectors floating around. Everything is held in this df
```
