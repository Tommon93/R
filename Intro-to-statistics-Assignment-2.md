Assignment 2
================

## Group/Individual Details

  - Thomas Le

## Executive Statement

The aim of this investigation is to determine whether Woolworths or
Coles have better pricing over the other. The products were sampled
through simple random sampling (SRS). A numbered list of products was
made on excel and using a random number generator, 33 items were
randomly selected based on the random number given. As per the central
limit theorem a size greater than 30 was sufficient to sample
considering time and resource constraints. The products were matched via
each website to ensure the same type of product, brand and weight or
volume were consistent. Prices appeared to be quite small, with items
being mostly under 13AUD. Any outliers would have skewed the mean,
therefore items were capped at 15AUD.

A paired t-test was used to find if there were any significant
difference between the mean prices of each supermarket. It was found
that there was significant difference and that Coles was cheaper to shop
at on average.

## Load Packages and Data

``` r
library(ggplot2)
library(readxl)
```

    ## Warning: package 'readxl' was built under R version 4.0.5

``` r
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
Prices <- read_xlsx("Book1.xlsx")
Prices2 <- read_xlsx("Book2.xlsx")
```

## Summary Statistics

A sample of 33 products were used to compare the summary statistics. It
appears the mean of Woolworths is slightly greater with a mean of
5.22AUD vs Coles; 4.70AUD.

``` r
Prices %>% group_by(Store) %>% summarise(Min = min(Prices, na.rm = TRUE),
                                          Q1 = quantile(Prices,probs = .25,na.rm = TRUE),
                                         Median = median(Prices, na.rm = TRUE),
                                         Q3 = quantile(Prices,probs = .75,na.rm = TRUE),
                                         Max = max(Prices,na.rm = TRUE),
                                         Mean = mean(Prices, na.rm = TRUE),
                                         SD = sd(Prices, na.rm = TRUE),
                                         n = n(),
                                         Missing = sum(is.na(Prices)))
```

    ## `summarise()` ungrouping output (override with `.groups` argument)

    ## # A tibble: 2 x 10
    ##   Store        Min    Q1 Median    Q3   Max  Mean    SD     n Missing
    ##   <chr>      <dbl> <dbl>  <dbl> <dbl> <dbl> <dbl> <dbl> <int>   <int>
    ## 1 Coles       1.47  2.5    4.25  6.35    13  4.71  2.66    33       0
    ## 2 Woolworths  1.15  2.95   5.1   7       13  5.23  2.87    33       0

Visual Comparison:

The boxplot showed that the IQR of Woolworth is slightly larger. There
is an ‘outlier’ for Coles, but the outlier and the max price are of the
same product from both supermarkets. It is shown as an ‘outlier’ for
Coles because the prices overall from Coles appear to be lower and
therefore products of 13AUD was seen as an outlier.

``` r
Prices %>% group_by(Store) %>% boxplot(Prices ~ Store, data = .,
                                      main = "Price Comparison of Woolworths and Coles",
                                      ylab = "Price in AUD",
                                      col = c("red", "green"))
```

![](Intro-to-statistics-Assignment-2_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

Price Variation/Difference:

Of the 33 sample products, We can see from the line plot more of the
price movement is moving down as it moves across to Coles. 15 products
had the same price.

  - A line plot was used as the data set was not large.

<!-- end list -->

``` r
matplot(t(data.frame(Prices2$Woolworth, Prices2$Coles)),
  type = "b",
  pch = 19,
  col = 1,
  lty = 1,
  xlab = " Price Movement",
  ylab = "Price AUD",
  xaxt = "n")
axis(1, at = 1:2, labels = c("Woolworths", "Coles"))
```

![](Intro-to-statistics-Assignment-2_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

## Hypothesis Test

The null hypothesis is that the product prices between Woolworths and
Coles have no signficance difference.

  - H0 : μΔ = 0
  - HA : μΔ ≠ 0 HA : μΔ \< 0 HA : μΔ \> 0

A paired sample t-test was performed to compare the mean prices of
Woolworths and Coles. A 95% Confidence Interval was used and a
significance level of p\<0.05. A paired t-test was used as we are
comparing the same product from two different population (Woolworths and
Coles).

It was assumed that the depend variable should be approximately follow a
normal distribution, as per the central limit theorem (n\>30). Each
product was different from each other, and the dependent variable
(price) does not contain outliers.

``` r
t.test(Prices2$Woolworth, Prices2$Coles, paired = TRUE, alternative = "two.sided")
```

    ## 
    ##  Paired t-test
    ## 
    ## data:  Prices2$Woolworth and Prices2$Coles
    ## t = 2.4296, df = 32, p-value = 0.02091
    ## alternative hypothesis: true difference in means is not equal to 0
    ## 95 percent confidence interval:
    ##  0.08413568 0.95707644
    ## sample estimates:
    ## mean of the differences 
    ##               0.5206061

## Interpretation

The results of the paired t-test showed that we should reject the null
hypothesis. It was found that there was significant difference between
the mean prices of Woolworths and Coles: t(df = 32) = 2.42, p \< 0.02,
with a price difference of 0.52AUD and 95%CI \[0.084, 0.957\]. It
appears from the test Woolworth’s prices are significantly higher than
the average Coles price.

## Discussion

From the investigation it was concluded that on average the prices at
Coles were lower than Woolworths.

Strengths:

  - Convenience to online access to data for the prices of both
    supermarkets

Limitations:

  - Unable to capture the whole population of both supermarkets
  - Smaller data set was used due to time constraints and working
    individually.
  - Some of the products were on specials or discounts from both
    supermarkets, but this was a more realistic approach as there are
    always different sales and discounts occuring from both supermarkets
    on a broad range of products.

Sampling could have been improved due to time and resource constraints.
The investigation overall concluded that at this time, Coles offers
better value for shoppers.
