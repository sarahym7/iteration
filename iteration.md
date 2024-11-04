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

    ##  [1]  1.13830013  0.18801052  0.72988489 -0.06349911 -1.48251801  0.95133522
    ##  [7]  1.05027172  0.43804489 -0.82358234 -1.00981510 -0.89158509 -2.21026178
    ## [13]  1.67608632 -1.04579894 -1.39562911  0.52811385  0.53878269 -0.35513807
    ## [19]  0.09524656  0.18723617 -1.41606286  1.19490931  1.21453866  1.14320106
    ## [25]  0.38994384 -0.59726052 -0.71314502 -0.67263019  1.02207021  0.19095007

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

    ##  [1]  1.13830013  0.18801052  0.72988489 -0.06349911 -1.48251801  0.95133522
    ##  [7]  1.05027172  0.43804489 -0.82358234 -1.00981510 -0.89158509 -2.21026178
    ## [13]  1.67608632 -1.04579894 -1.39562911  0.52811385  0.53878269 -0.35513807
    ## [19]  0.09524656  0.18723617 -1.41606286  1.19490931  1.21453866  1.14320106
    ## [25]  0.38994384 -0.59726052 -0.71314502 -0.67263019  1.02207021  0.19095007

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
    ## 1  3.69  3.88

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
    ## 1  4.20  3.19

Translating into a function

``` r
sim_mean_sd= function(sample_size, mu = 3, sigma =4) {  # also provide default values, but we can overwrite it
  
  
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
    ## 1  6.35  3.06

``` r
# r is assuming first number is the first argument, then second etc 


# we can also name match 

sim_mean_sd(sample_size = 100,mu =  6, sigma = 3) # positional matching 
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.65  2.92

``` r
sim_mean_sd(mu = 6, sample_size = 100, sigma = 3) # another example 
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  6.00  3.12

``` r
sim_mean_sd(sample_size = 100) # we dont give it the values of the others so will rely on the default
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.27  4.00
