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
    ## -2.92431 -0.65098  0.12950 -0.01479  0.66406  2.08116

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

    ##  [1] 1.963014 4.680575 4.161709 3.613050 4.015153 1.231735 1.727803 3.553507
    ##  [9] 1.851953 3.726848 3.900536 3.555980 3.173791 1.081540 2.328255 3.796116
    ## [17] 2.202008 2.914069 4.340414 3.154573

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
    ## 1  3.05  1.07

``` r
mean_and_sd(list_norm[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.266  5.26

``` r
mean_and_sd(list_norm[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.99 0.184

``` r
mean_and_sd(list_norm[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.79 0.800

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
    ## 1  3.05  1.07
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.266  5.26
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.99 0.184
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.79 0.800

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
    ##  [1] 1.963014 4.680575 4.161709 3.613050 4.015153 1.231735 1.727803 3.553507
    ##  [9] 1.851953 3.726848 3.900536 3.555980 3.173791 1.081540 2.328255 3.796116
    ## [17] 2.202008 2.914069 4.340414 3.154573
    ## 
    ## $b
    ##  [1]  -4.7062805  -2.7504116   4.8362612  -2.6407903  -1.2634821   2.2505681
    ##  [7]  -7.6101479   1.2647198  -4.8625563   4.0685146 -10.1038419   4.1555797
    ## [13]   6.0946407  -3.6234747   1.6805829   1.3980995   5.0274426  -6.0000585
    ## [19]  -2.9556113  -3.6606565  10.3264762  -3.7695090  -5.1398956   8.1746120
    ## [25]   0.8292821   7.0458648   1.7474417   1.3154479  -3.1893107  10.0409108
    ## 
    ## $c
    ##  [1]  9.741803 10.041420 10.131174  9.888236 10.056533  9.940628  9.963945
    ##  [8]  9.855036  9.881343 10.049441  9.986472 10.062477  9.945590  9.815426
    ## [15]  9.760828 10.359718  9.830691  9.984992 10.350904  9.963145 10.041039
    ## [22]  9.860990  9.945682 10.007715  9.890001 10.175776  9.859951 10.008356
    ## [29]  9.972655 10.035574 10.478204  9.921486 10.034847 10.266833 10.026959
    ## [36]  9.667961  9.720578 10.375036  9.748507  9.921327
    ## 
    ## $d
    ##  [1] -2.418500 -3.683673 -1.815983 -3.784964 -2.180809 -2.098327 -2.362469
    ##  [8] -2.748937 -2.571300 -2.689784 -3.934413 -4.028429 -3.971411 -1.402400
    ## [15] -2.484413 -1.966523 -2.198897 -3.735164 -2.844824 -2.936743

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
    ##  [1] 1.963014 4.680575 4.161709 3.613050 4.015153 1.231735 1.727803 3.553507
    ##  [9] 1.851953 3.726848 3.900536 3.555980 3.173791 1.081540 2.328255 3.796116
    ## [17] 2.202008 2.914069 4.340414 3.154573
    ## 
    ## $b
    ##  [1]  -4.7062805  -2.7504116   4.8362612  -2.6407903  -1.2634821   2.2505681
    ##  [7]  -7.6101479   1.2647198  -4.8625563   4.0685146 -10.1038419   4.1555797
    ## [13]   6.0946407  -3.6234747   1.6805829   1.3980995   5.0274426  -6.0000585
    ## [19]  -2.9556113  -3.6606565  10.3264762  -3.7695090  -5.1398956   8.1746120
    ## [25]   0.8292821   7.0458648   1.7474417   1.3154479  -3.1893107  10.0409108
    ## 
    ## $c
    ##  [1]  9.741803 10.041420 10.131174  9.888236 10.056533  9.940628  9.963945
    ##  [8]  9.855036  9.881343 10.049441  9.986472 10.062477  9.945590  9.815426
    ## [15]  9.760828 10.359718  9.830691  9.984992 10.350904  9.963145 10.041039
    ## [22]  9.860990  9.945682 10.007715  9.890001 10.175776  9.859951 10.008356
    ## [29]  9.972655 10.035574 10.478204  9.921486 10.034847 10.266833 10.026959
    ## [36]  9.667961  9.720578 10.375036  9.748507  9.921327
    ## 
    ## $d
    ##  [1] -2.418500 -3.683673 -1.815983 -3.784964 -2.180809 -2.098327 -2.362469
    ##  [8] -2.748937 -2.571300 -2.689784 -3.934413 -4.028429 -3.971411 -1.402400
    ## [15] -2.484413 -1.966523 -2.198897 -3.735164 -2.844824 -2.936743

``` r
listcol_df$samp[[1]] 
```

    ##  [1] 1.963014 4.680575 4.161709 3.613050 4.015153 1.231735 1.727803 3.553507
    ##  [9] 1.851953 3.726848 3.900536 3.555980 3.173791 1.081540 2.328255 3.796116
    ## [17] 2.202008 2.914069 4.340414 3.154573

``` r
# we want mean and sd 


mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.05  1.07

What if we want to apply it all the way through. Can’t we just map?

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.05  1.07
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.266  5.26
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.99 0.184
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.79 0.800

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

# we're not losing information and have vectors floating around. Everything is held in this df. Everything is tibbled so good. Now look at an example that is less contrived since we made up the data. 
```

Using the weather data

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USC00519397", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2017-01-01",
    date_max = "2017-12-31") |>
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Molokai_HI",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) |>
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## using cached file: C:\Users\sarah\AppData\Local/R/cache/R/rnoaa/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2024-09-26 23:39:45.766142 (8.668)

    ## file min/max dates: 1869-01-01 / 2024-09-30

    ## using cached file: C:\Users\sarah\AppData\Local/R/cache/R/rnoaa/noaa_ghcnd/USC00519397.dly

    ## date created (size, mb): 2024-10-30 14:21:01.099861 (1.71)

    ## file min/max dates: 1965-01-01 / 2023-09-30

    ## using cached file: C:\Users\sarah\AppData\Local/R/cache/R/rnoaa/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2024-09-26 23:40:17.640862 (1.038)

    ## file min/max dates: 1999-09-01 / 2024-09-30

Get our list columns

``` r
weather_nest = 
  weather_df %>% 
  nest(data = date:tmin)
  
  
  #nest some of the observations in the dataset, we need to create columns we want to nest within the columns that remain. We will create new column called data and it will result from nesting everything from date to tmin
  # notice name stays the same, data is a tibble with 365 rows and one for everyday of the year, 4 columns for data, precip, tmax,, tmin
```

``` r
weather_nest %>%  pull(name)
```

    ## [1] "CentralPark_NY" "Molokai_HI"     "Waterhole_WA"

``` r
weather_nest %>% pull(data)
```

    ## [[1]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # ℹ 355 more rows
    ## 
    ## [[2]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0  26.7  16.7
    ##  2 2017-01-02     0  27.2  16.7
    ##  3 2017-01-03     0  27.8  17.2
    ##  4 2017-01-04     0  27.2  16.7
    ##  5 2017-01-05     0  27.8  16.7
    ##  6 2017-01-06     0  27.2  16.7
    ##  7 2017-01-07     0  27.2  16.7
    ##  8 2017-01-08     0  25.6  15  
    ##  9 2017-01-09     0  27.2  15.6
    ## 10 2017-01-10     0  28.3  17.2
    ## # ℹ 355 more rows
    ## 
    ## [[3]]
    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01   432  -6.8 -10.7
    ##  2 2017-01-02    25 -10.5 -12.4
    ##  3 2017-01-03     0  -8.9 -15.9
    ##  4 2017-01-04     0  -9.9 -15.5
    ##  5 2017-01-05     0  -5.9 -14.2
    ##  6 2017-01-06     0  -4.4 -11.3
    ##  7 2017-01-07    51   0.6 -11.5
    ##  8 2017-01-08    76   2.3  -1.2
    ##  9 2017-01-09    51  -1.2  -7  
    ## 10 2017-01-10     0  -5   -14.2
    ## # ℹ 355 more rows

``` r
weather_nest$data[[1]]  
```

    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # ℹ 355 more rows

Suppose I want to regress `tmax` on `tmin` for each station is tmax
predicted by tmin for each station? We can do regression for a dataset
and can we do it separately

``` r
weather_nest$data[[1]]
```

    ## # A tibble: 365 × 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # ℹ 355 more rows

``` r
# if what we want to do is regress weather_nest tmax on tmin and the dataset we are interested is weather_nest df1 using linear model command to regress

lm(tmax~tmin, data = weather_nest$data[[3]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nest$data[[3]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

We want a function that takes the input into an arguement

``` r
weather_lm = function(df){
  
  lm(tmax ~tmin, data =df)
  
}

# we have a function that given an input elements of a list computes a linear model and spits the result 

weather_lm(weather_nest$data[[1]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039

``` r
weather_lm(weather_nest$data[[2]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509

``` r
weather_lm(weather_nest$data[[3]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

``` r
weather_lm = function(df){
  
  lm(tmax ~tmin, data =df)
  
}

output = vector("list", 3) 
  for (i in 1:3) {
    
   output[[i]] = weather_lm(weather_nest$data[[i]])
    
  }
```

What about map?

``` r
map(weather_nest$data , weather_lm) # a list where each element is a dataset and we want to do a map across the list 
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

What about a map in a list colulmn?

``` r
weather_nest = 
weather_nest %>% 
  mutate( models = map(data, weather_lm))
 #we are adding a column to store the results of linear modeling. Need to map across a column and apply the function weather_lm . Don't have to do weather_nest its implied by the tidy verse process and you are sitting within a dataframe. Now, we get a new df with names, id, data, models, 



weather_nest$models 
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

``` r
#this is telling us that if we have data nested within stations of 1,2,3 we can start doing linear models. 
```
