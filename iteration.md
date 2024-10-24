Iteration
================
Sarahy Martinez
2024-10-24

``` r
library(tidyverse)
library(rvest)

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

## Do something Simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec))/ sd(x_vec)  # these are z scores now, kind of thing we cant to do in a body of function
```

    ##  [1] -0.2852012 -0.6738401  0.5020353 -0.4167547  1.0015668 -0.3832419
    ##  [7]  2.1328568 -1.7373453  0.6938268  0.3477105 -0.3063864  0.8786643
    ## [13]  0.3047099 -1.3388047 -0.7391574 -1.4714097 -0.2662500 -1.2164694
    ## [19]  0.1540060 -0.8265098  0.7213243 -0.1713973  0.9943462  1.4671600
    ## [25]  1.0188967  1.9864745 -1.3828940 -0.3483732 -0.1129375 -0.5266053

I want a function to compute z scores

``` r
#inside function we only want to use something named x, so use where you want to operate

z_scores = function(x){
z =  ( x - mean(x))/sd(x)
   
return(z)
}


# telling z scores input is the x_vec

z_scores(x_vec)    #same answer as function above! 
```

    ##  [1] -0.2852012 -0.6738401  0.5020353 -0.4167547  1.0015668 -0.3832419
    ##  [7]  2.1328568 -1.7373453  0.6938268  0.3477105 -0.3063864  0.8786643
    ## [13]  0.3047099 -1.3388047 -0.7391574 -1.4714097 -0.2662500 -1.2164694
    ## [19]  0.1540060 -0.8265098  0.7213243 -0.1713973  0.9943462  1.4671600
    ## [25]  1.0188967  1.9864745 -1.3828940 -0.3483732 -0.1129375 -0.5266053

``` r
# update your function, using conditional execution 


z_scores = function(x) {
  
  if (!is.numeric(x)) {
    stop("Argument x should be numeric")
  } 
  
  if (length(x) <3) {
  stop("Input must have at least 3 numbers")
    
  }
  
z =  ( x - mean(x))/sd(x)

return(z)
  
}
```

Try my function on some other thing they should give errors

``` r
z_scores(3) #returns NA bc we need more numbers than 1 number to compute sd, updated conditions above and needs 3 numbers
```

    ## Error in z_scores(3): Input must have at least 3 numbers

``` r
z_scores("jeff") #can't take mean of a character
```

    ## Error in z_scores("jeff"): Argument x should be numeric

``` r
z_scores(mtcars) # you can't take the mean of a dataset, only list of numbers
```

    ## Error in z_scores(mtcars): Argument x should be numeric

``` r
z_scores(c(TRUE, TRUE, FALSE,TRUE))  #works bc will coerce to 0 and 1s 
```

    ## Error in z_scores(c(TRUE, TRUE, FALSE, TRUE)): Argument x should be numeric

## Multiple Outputs

``` r
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("Argument x should be numeric")
  } 
  
  if (length(x) <3) {
  stop("Input must have at least 3 numbers")
    
  }
  
mean_x = mean(x)   # instead of computing mean and sd , we want both
sd_x = sd(x)

list(mean = mean_x,
    sd= sd_x)

}


#Alternatively, we might store values in a data frame.

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

Check if the functions work

``` r
#x_vec = rnorm(100) #we can change the vector and compute its sd 

x_vec = rnorm(100, mean = 3, sd=4)

mean_and_sd(x_vec)  # we get the mean and std deviation 
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.69  3.80

## Multiple Inputs

If wanted to run with different sample sizes, means, and standard
deviations. So, give me the mean and sd and spit things out

``` r
sim_data=
  tibble(
    
    x= rnorm(100, mean = 4, sd =3)  # creating a tibble (table of values), with diff mean and SD
  )

sim_data %>% 
  summarize(
    mean = mean(x),
    sd = sd(x)
  )
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  4.17  2.81

Translating into a function

``` r
sim_mean_sd= function(sample_size, mu, sigma) {
  
  
  sim_data=
    
  tibble(
    
    x= rnorm(n= sample_size, mean = mu, sd=sigma)  # creating a tibble (table of values), with diff mean and SD
  )

  
sim_data %>% 
  summarize(
    mean = mean(x),
    sd = sd(x)
  )

}



sim_mean_sd(100, 6, 3)  #if we run code multiple times will create a diff mean, st etc. we can learn about process expecting how much the mean will shift from true. 
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  6.09  2.94
