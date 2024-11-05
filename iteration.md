Iteration
================
Sarahy Martinez
2024-10-24

``` r
library(tidyverse)
library(rvest)
library(stringr)
library(tibble)

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

    ##  [1]  1.20624667  1.85966525  0.01848997  2.06198939 -1.05976314  0.47909984
    ##  [7] -1.16622956 -0.22388832  0.22466539 -1.41284717  0.17237623 -0.58245993
    ## [13] -0.02292778 -1.24429900  0.07976235 -0.73457782  1.89281996 -0.35561902
    ## [19] -1.17378989 -1.36800208  0.23110993 -0.38724064 -0.54885782 -0.59757416
    ## [25] -1.07633997  0.85084099  1.37500412  0.14408864  0.56767558  0.79058197

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

    ##  [1]  1.20624667  1.85966525  0.01848997  2.06198939 -1.05976314  0.47909984
    ##  [7] -1.16622956 -0.22388832  0.22466539 -1.41284717  0.17237623 -0.58245993
    ## [13] -0.02292778 -1.24429900  0.07976235 -0.73457782  1.89281996 -0.35561902
    ## [19] -1.17378989 -1.36800208  0.23110993 -0.38724064 -0.54885782 -0.59757416
    ## [25] -1.07633997  0.85084099  1.37500412  0.14408864  0.56767558  0.79058197

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
    ## 1  2.97  3.95

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
    ## 1  4.06  2.62

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
    ## 1  5.37  3.02

``` r
# r is assuming first number is the first argument, then second etc 


# we can also name match 

sim_mean_sd(sample_size = 100,mu =  6, sigma = 3) # positional matching 
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  6.11  2.76

``` r
sim_mean_sd(mu = 6, sample_size = 100, sigma = 3) # another example 
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.80  3.20

``` r
sim_mean_sd(sample_size = 100) # we dont give it the values of the others so will rely on the default
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.06  4.37

# Lets review Napolean Dynamite

GOAL: Go over to amazon and get customer reviews of napolean dynamite
and gather it as data. Ie. reading data from the web using rvest

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviwerType=avp_only_reviews&sortBy=recent&pageNumber=1"

dynamite_html = read_html(url)

review_titles =
  dynamite_html %>% 
  html_nodes(".a-text-bold span") %>% # identified css tags using selectr gadget and will give tag to get the data we want
  html_text() # converted to text

review_stars =
   dynamite_html %>% 
  html_nodes("#cm_cr-review_list .review-rating") %>% 
  html_text() %>% 
  str_extract("^\\d") %>%  #says to get the first digit number between 0 and 9
  as.numeric() # converted to numeric


review_text = 
    dynamite_html %>% 
  html_nodes(".review-text-content span") %>% 
  html_text() %>% 
  str_replace_all("\n","") %>%  #trimming off additional spaces 
  str_trim()
  

reviews = tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)
```

What about the next page of reviews?

``` r
# page 2 

url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviwerType=avp_only_reviews&sortBy=recent&pageNumber=2"

dynamite_html = read_html(url)

review_titles =
  dynamite_html %>% 
  html_nodes(".a-text-bold span") %>% # identified css tags using selectr gadget and will give tag to get the data we want
  html_text() # converted to text

review_stars =
   dynamite_html %>% 
  html_nodes("#cm_cr-review_list .review-rating") %>% 
  html_text() %>% 
  str_extract("^\\d") %>%  #says to get the first digit number between 0 and 9
  as.numeric() # converted to numeric


review_text = 
    dynamite_html %>% 
  html_nodes(".review-text-content span") %>% 
  html_text() %>% 
  str_replace_all("\n","") %>%  #trimming off additional spaces 
  str_trim()
  

reviews = tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)
```

we don’t want to copy and past and repeat everything twice. It will be
annoying. So we will do something different. Lets turn this code into a
function. The only input we have that changes is the pages,so whats the
structure? Here its just the URL.

``` r
read_page_reviews = function(url){ #instead of copying we want give a url, read html, get rev title, star text and return obj
html = read_html(url)

review_titles =
  html %>% 
  html_nodes(".a-text-bold span") %>% # identified css tags using selectr gadget and will give tag to get the data we want
  html_text() # converted to text

review_stars =
  html %>% 
  html_nodes("#cm_cr-review_list .review-rating") %>% 
  html_text() %>% 
  str_extract("^\\d") %>%  #says to get the first digit number between 0 and 9
  as.numeric() # converted to numeric


review_text = 
  html %>% 
  html_nodes(".review-text-content span") %>% 
  html_text() %>% 
  str_replace_all("\n","") %>%  #trimming off additional spaces 
  str_trim()
  

reviews = 
  tibble(
    title = review_titles,
    stars = review_stars,
    text = review_text
)

reviews #(output)

}
```

Lets try the function.

``` r
dynamite_url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviwerType=avp_only_reviews&sortBy=recent&pageNumber=2"

read_page_reviews(dynamite_url)
```

    ## # A tibble: 0 × 3
    ## # ℹ 3 variables: title <chr>, stars <dbl>, text <chr>

``` r
#we dont have to copy the code and can pass whenever we need to or want to add another element. We can make a change inside of the function
```

Lets read a few pages of reviews. - setting stage for next vids - lets
suppose we have the dynamite URL base. Will be everything upto the page
we are insterested in. - combine the url base with the numbers 1-5

``` r
dynamite_url_base = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviwerType=avp_only_reviews&sortBy=recent&pageNumber="

dynamite_urls = str_c(dynamite_url_base, 1:5) # dont love that its a vector but if we want the first page 
dynamite_urls[[1]]

# so instead we can do this 
# intermediate spot to say we've got a function and the function works but we should see if we want to do it again for 50 we dont want to write it down 50 times 

all_reviews = 
bind_rows(
read_page_reviews(dynamite_urls[1]),
read_page_reviews(dynamite_urls[2]),
read_page_reviews(dynamite_urls[3]),
read_page_reviews(dynamite_urls[4]),
read_page_reviews(dynamite_urls[5])
)
```
