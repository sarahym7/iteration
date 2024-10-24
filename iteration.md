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

    ##  [1] -0.64006851  0.05707567  0.41459930 -0.39204501 -1.65087203 -0.22689116
    ##  [7] -0.42253866  0.77407277 -0.27202104  1.00020837  0.80220933  0.02353142
    ## [13]  0.36147510  1.78717159 -0.93316650  0.74610198 -1.70805423 -1.75478189
    ## [19] -1.28827117  0.55216267  0.34778772  0.45121651 -0.62787910 -1.69038888
    ## [25]  0.72486590 -0.41516656  0.97160346  1.98133420  1.06302080 -0.03629204

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

    ##  [1] -0.64006851  0.05707567  0.41459930 -0.39204501 -1.65087203 -0.22689116
    ##  [7] -0.42253866  0.77407277 -0.27202104  1.00020837  0.80220933  0.02353142
    ## [13]  0.36147510  1.78717159 -0.93316650  0.74610198 -1.70805423 -1.75478189
    ## [19] -1.28827117  0.55216267  0.34778772  0.45121651 -0.62787910 -1.69038888
    ## [25]  0.72486590 -0.41516656  0.97160346  1.98133420  1.06302080 -0.03629204

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
