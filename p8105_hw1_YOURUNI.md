p8105_hw1_sp4170.rmd
================
Shihui Peng
2023-09-24

# Problem 1

I load the moderndive library, and load the early_january_weather
dataset:

## Description for dataset

- The data set early_january_weather includes several variables.
  - Their names are **origin, year, month, day, hour, temp, dewp, humid,
    wind_dir, wind_speed, wind_gust, precip, pressure, visib,
    time_hour**.
  - There are **358 rows** and **15 columns**.
  - Within the obvervation range of this dataset, the mean temperature
    is **39.5821229 Fahrenheit**.

## Scatterplot and description

### Scatterplot

I make a scatterplot of **temp (y)** vs **time_hour (x)**; For **humid
variable**, I use color points.

``` r
plot_hw1 = ggplot(data=early_january_weather, aes(x = time_hour, y = temp, color = humid))+ geom_point()
plot_hw1
```

![](p8105_hw1_YOURUNI_files/figure-gfm/scatterplot%20for%20time_hour%20&%20temp-1.png)<!-- -->

### Description

Based on the scatterplot, we can find that within the database
observation range, the temperature generally shows an upward trend. The
daily temperature fluctuates. The humidity shows an upward trend, too.

### Export my scatterplot

Then, I export my scatterplot to my project directory and name it
“scatterplot_hw1.pdf” in a format of PDF.

``` r
ggsave("scatterplot_hw1.pdf",plot_hw1)
```

    ## Saving 7 x 5 in image

# Problem 2

## Create a data frame

- I create a data frame comprised of:

  - a random sample of size 10 from a standard Normal distribution
  - a logical vector indicating whether elements of the sample are
    greater than 0
  - a character vector of length 10
  - a factor vector of length 10, with 3 different factor “levels”

``` r
hw1_df = 
  tibble(
    ran_sam = rnorm(10),
    log_vec = ran_sam > 0,
    char_vec = c("a", "b", "c", "d", "e", "f", "g", "h", "i", "j"),
    fac_vec = factor(sample(c("level1", "level2", "level3"), size = 10, replace = TRUE))
  )
hw1_df
```

    ## # A tibble: 10 × 4
    ##    ran_sam log_vec char_vec fac_vec
    ##      <dbl> <lgl>   <chr>    <fct>  
    ##  1   0.584 TRUE    a        level3 
    ##  2  -0.794 FALSE   b        level3 
    ##  3  -0.487 FALSE   c        level3 
    ##  4   0.141 TRUE    d        level2 
    ##  5   0.194 TRUE    e        level3 
    ##  6   0.333 TRUE    f        level1 
    ##  7   0.107 TRUE    g        level3 
    ##  8  -2.18  FALSE   h        level2 
    ##  9  -0.806 FALSE   i        level3 
    ## 10   0.902 TRUE    j        level2

## Try to take the means

I try to take the mean of each variable in your dataframe by pulling
those variables out of my data frame hw1_df. Tidyverse library has been
loading from the very beginning so no need to load it again here.

``` r
mean_ran_sam = hw1_df %>% pull(ran_sam) %>% mean()
mean_log_vec = hw1_df %>% pull(log_vec) %>% mean()
mean_char_vec = hw1_df %>% pull(char_vec) %>% mean()
```

    ## Warning in mean.default(.): argument is not numeric or logical: returning NA

``` r
mean_fac_vec = hw1_df %>% pull(fac_vec) %>% mean()
```

    ## Warning in mean.default(.): argument is not numeric or logical: returning NA

``` r
mean_ran_sam
```

    ## [1] -0.2010774

``` r
mean_log_vec
```

    ## [1] 0.6

``` r
mean_char_vec
```

    ## [1] NA

``` r
mean_fac_vec
```

    ## [1] NA

When trying to take the mean of each variable in my dataframe using pull
function, the mean for my **random sample** and **logical vector** work,
but the mean for my **character vector** and **factor vector** don’t
work.

## Convert variables

``` r
log_num = as.numeric(hw1_df$log_vec)
char_num = as.numeric(hw1_df$char_vec)
```

    ## Warning: NAs introduced by coercion

``` r
fac_num = as.numeric(hw1_df$fac_vec)
```

- when using, we try to convert logical, character, and factor variables
  to numeric variables.
  - **Logical values** are coerced to numeric values, where TRUE becomes
    1 and FALSE becomes 0. Therefore, when using pull to calculate the
    mean of our logical data, we can get a result. So this explain what
    has happened above when try to get the mean of logical vectors set.
  - r tries to convert **character values** into numeric, however, it
    can only get not available output (NA). Therefore, when using pull
    to calculate the mean of our character data, we fail to get a
    result. This can explain what has happened above when try to get the
    mean of our character vectors set.
  - although the **factor vector** set is regarded as integer, it has
    its meaning of level. Therefore, when converting, we can get numeric
    vectors containing the integers representing the levels. However,
    when we use pull function, we cannot get the mean of our factor
    vectors set, but we can get the mean of this set when we use
    as.numeric function. This might be because the actual factor levels
    would not be directly converted to their original integer
    representations. They would be converted to numeric vectors with
    integers corresponding to the levels. So when we simply uss pull
    function to calculate the mean of a factor vector, we would get NA
    because we are calculates the mean of the underlying integers
    representing the factor levels, not the mean of the original factor
    levels themselves. This can explain why we cannot get the mean when
    using pull function but can get the mean when we convert the factor
    vectors into numeric ones.
