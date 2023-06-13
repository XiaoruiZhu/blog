---
title: "Summary Measures in Business Analytics"
excerpt: "Summary Measures in Business Analytics: location measures, measures of dispersion, shape, and association; boxplots and z scores."
categories: articles
tags: [Statistics, Business Analytics, R]
comments: true
share: true
date: 2023-06-12 11:10:13
---

# Introductory Case: Marketing Analysis

Summary measures in Business Analytics are very important. They are the measures of location, dispersion, shape, and association. They measure different aspects of the variable. These measures help analyst to release the information or characteristics contained in the data. Then, the characteristics may provide some insightful understandings of the data. With the insights, decision makers can better understand the problems or make better decision. For example, a marketing analyst of an online retail company has collected some data of their customers' spending. She wants to understand spending behavior of customers so the company can operate the inventory better, dynamically adjust the price of products, giving out promotion deals to the relevant customers.

A few tasks:

1.  What you should do to gain understanding of the typical spending for these categories.
2.  How to gain dispersion of the spending for these categories.
3.  Should the manager target the male and female customer differently.

## Measures of Location

The central location reveals the middle or center of the data. It is a numerical value that attempts to describe the center of variable's distribution.

There a re few measures: - mean: arithmetic mean is the primary one. - median: middle value of the sorted data. There is equal number of data points that lie above and below it.\
- mode: the most frequently occurred observation. It is useful for a categorical variable. - percentile: A measure of relative position. For example, the $p$-th percentile divides a variable into two parts. Approximately $p$ percent of the observations are less than the $p$th percentile. Approximately (100 − $p$) percent of the observations are greater than the $p$th percentile

> Try to ask [ChatGPT](https://chat.openai.com/): "What is the measure of location in statistics?"

### Example

1.  What you should do to gain understanding of the typical spending for these categories. Assessing the average spending for each of the product categories can help with this.

Question: suppose you want to estimate the total spending of clothing for all your 100,000 customer during this holiday season, how to get a rough guess about the total amount of revenue?

``` r
library(readxl)
myData <- read_excel("jaggia_ba_2e_ch03_data.xlsx", 
    sheet = "Online")
summary(myData)

median(myData$Clothing)
# Calculate the 10th percentile
quantile(myData$Clothing, probs = 0.1)

# Answer to the question above: 
100000 * mean(myData$Clothing)
```

> If you don't fully understand how to use quantile() function, try to ask ChatGPT: "how to use quantile() function in R?"

## Measures of Dispersion

Dispersion measures gauge the variability of the variable.

Two key features: 
- 0 indicates all the observations are identical.
- Increases as the observations become more diverse.

A few common measures of dispersion: 

1. Range: The simplest measure that take the difference between the maximum and minimum. (Not good since it is sensitive to extreme values)

``` r
range(myData$Clothing)
diff(range(myData$Clothing))
```

2. Interquartile Range (IQR): The difference between the third quantile (Q3) and the first quantile (Q1). It is less sensitive to extreme values compared to the range measure. It mainly measure the range of the middle 50% of the data. 

``` r
IQR(myData$Clothing)
```

3. Variance: The average of the squared deviations from the mean. It measures how far each value in the dataset is from the mean and provides an estimate of the average squared deviation.

``` r
var(myData$Clothing)
```

4. Standard Deviation: The square root of the variance. It measures the average distance between each data point and the mean and is widely used due to its intuitive interpretation and connection to the original units of the data.

``` r
sd(myData$Clothing)
# or 
sqrt(var(myData$Clothing))
```

5. Mean Absolute Deviation (MAD): The average of the absolute deviations from the mean. It provides a measure of the average absolute distance of each data point from the mean and is less sensitive to extreme values compared to the standard deviation.

``` r
# Calculate the mean of the data
mean_value <- mean(myData$Clothing)

# Calculate the absolute deviations
absolute_deviations <- abs(myData$Clothing - mean_value)

# Calculate the Mean Absolute Deviation (MAD)
mad_value <- mean(absolute_deviations)

# Print the calculated MAD
print(mad_value) 
# or just
mad_value
```

> What if you ask ChatGPT: "how to calculate Mean Absolute Deviation in R?"

6. Coefficient of Variation (CV): The ratio of the standard deviation to the mean, expressed as a percentage. It provides a relative measure of dispersion that allows for comparison of variability between datasets with different scales.

``` r
# Calculate the standard deviation of the data
sd_value <- sd(myData$Clothing)

# Calculate the Coefficient of Variation (CV) using standard deviation and mean_value
cv_value <- (sd_value / mean_value) * 100

cv_value
```

> Try to ask [ChatGPT](https://chat.openai.com/): "what is the dispersion measure in statistics?"

## Measures of Shape

Shape measures show whether the distribution of the variable is symmetric or if the tails are more or less extreme than the normal distribution. It provides insights into the pattern or structure of the data distribution beyond just its central tendency and variability.

Common measures of shape in statistics include:

1. Skewness: Skewness measures the asymmetry of the data distribution. Positive skewness indicates a longer or fatter tail on the right side of the distribution, while negative skewness indicates a longer or fatter tail on the left side. A skewness value of 0 suggests a symmetric distribution.

``` r
# Calculate the skewness of a variable
install.packages("e1071")
library(e1071)

skewness(myData$Clothing)
```

2. Kurtosis: Kurtosis measures the heaviness of the tails of the data distribution. Positive kurtosis (leptokurtic) indicates heavy tails and a peaked central region, while negative kurtosis (platykurtic) indicates light tails and a relatively flat central region. A kurtosis value of 3 (excess kurtosis) is often used as a reference point for comparison with a normal distribution.

3. Modality: Modality refers to the number of modes or peaks in a distribution. A distribution can be unimodal (one peak), bimodal (two peaks), multimodal (more than two peaks), or uniform (no clear peaks).


## Measures of Association

Linear relationship: whether two numeric variables have a linear relationship.

1. Covariance: Covariance measures the joint variability between two variables. It indicates the extent to which changes in one variable are associated with changes in another variable. However, it doesn't provide a standardized measure of association like correlation does.

``` r
# Covariance between spending on clothing and Health 
cov(myData$Clothing, myData$Health)
```

2. Correlation: Correlation measures the strength and direction of the linear relationship between two continuous variables. It ranges from -1 to 1, where -1 indicates a perfect negative linear relationship, 0 indicates no linear relationship, and 1 indicates a perfect positive linear relationship. Correlation is useful for understanding the degree to which two variables move together.

``` r
# Correlation between spending on clothing and Health 
cor(myData$Clothing, myData$Health)
```

> Try to ask ChatGPT: "What is the measure of Association in business analytics?"
