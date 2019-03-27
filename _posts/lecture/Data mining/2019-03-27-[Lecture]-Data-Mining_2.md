---
layout: post
title:  "[Lecture] Data Mining_2"
image: ''
date:   2019-03-27 16:16:27
tags:
- Data mining
description: 'Data Mining'
categories:
- Lectures

---

This post is based on the lecture "Data mining" of professor Cho Sung Bae from Yonsei univ.

------

`03.18 Monday`

## Data visualization

It is very useful because it helps **understanding** the data structure, **cleaning** the data, identifying **outliers**, discover **correlations** among variables, and generating **interesting questions**. Graphs for data exploration can be divided 3 big parts: **Basic plots, Distribution plots, and Special structures**. 

<br>

### Basic plots

Line graph is used for time series. 

``` R
Amtrak.df <- read.csv("02. Amtrak.csv")
ridership.ts <- ts(Amtrak.df$Ridership, start = c(1991, 1), end = c(2004, 3), freq = 12)
plot(ridership.ts, xlab = "Year", ylab = "Ridership (in 000s)", ylim = c(1300, 2300)))
```

Bar chart is used for categorical vaiables

```R
housing.df <- read.csv("01. BostonHousing.csv")
data.for.plot <- aggregate(housing.df$MEDV, by = list(housing.df$CHAS), FUN = mean)
names(data.for.plot) <- c("CHAS", "MeanMEDV")
barplot(data.for.plot$MeanMEDV, names.arg = data.for.plot$CHAS, xlab = "CHAS", ylab = "Avg. MEDV")
```

![K-001](\assets\img\datamining\K-001.png)

Scatter plots display relationship between 2 numerical variables.

```R
plot(housing.df$MEDV ~ housing.df$LSTAT, xlab = "MEDV", ylab = "LSTAT")
```

### Distribution plots

Histogram shows the distribution of the variable.

```R
hist(housing.df$MEDV, xlab="MEDV")
```

Box plot shows min, Q1, median, Q3, max, mean, and outliers.

```R
par(mfcol = c(1,4)) # divide plot screen in to (1,4)
boxplot(housing.df$NOX ~ housing.df$CAT..MEDV, xlab = "CAT.MEDV", ylab="NOX")
```

Heat map is the correlation table. 

```R
heatmap(cor(housing.df), Rowv= NA, Colv =NA)
```

Matrix plot shows all the pairwise correlations between variables.

```R
plot(housing.df[, c(1, 3, 12, 13)])
```

<br>

## Data manipulation

### Rescaling

```R
plot(housing.df$MEDV ~ housing.df$CRIM, xlab = "CAT.MEDV", ylab="NOX")
plot(housing.df$MEDV ~ housing.df$CRIM, xlab = "CAT.MEDV", ylab="NOX", log = 'xy')
```

### Aggregation

tslm() function fit linear model to time series, including trend and seasonality components.

```R
library(forecast)
Amtrak.df <- read.csv("02. Amtrak.csv")
ridership.ts <- ts(Amtrak.df$Ridership, start = c(1991, 1), end = c(2004, 3), freq = 12)
ridership.lm <- tslm(ridership.ts ~ trend + I(trend^2))
plot(ridership.ts, xlab = "Year", ylab = "Ridership (in 000s)", ylim = c(1300, 2300))
lines(ridership.lm$fitted, lwd = 2)

```

monthly, yearly average is possible. 

### Scaling

smaller markers, jittering, color contrast. When there are same values, jitter() add small noise, to remove overlapping. 

<br>

---

`03.25 Monday`

## Dimension reduction

The goal of dimension reduction is **to remove redundant variables**, and **to combine similar categories**, because  useless variables can increase costs. There are 4 methods for dimension reduction.

1. Using the domain knowledges.
2. Data summaries
3. Categorical variables to numerical variables
4. PCA (Principal Component Analysis)

### Curse of dimensionality

Adding a variable increases exponentially to construct model. New variables make the data space sparse: which means insufficient data. So reduce the dimensionality is useful, a.k.a "factor selection" or "feature extraction".

<br>

## Data summaries

Use statical summary of data. If 2 variables have high correlation between themselves, then one of them can be reduced. 

You can use **Pivot tables**. It is interactive tables to combine information from multiple variables.

![example of pivot tables](https://www.excel-easy.com/data-analysis/images/pivot-tables/sorted-pivot-table.png)

You can also use **correlation analysis**. As I mentioned, strong correlation contains a lot of overlap in information. 

**Reducing categories** is also possible. A single categorical variable with *m* categories is transformed into *m-1* dummy variables. Problem of this point is, that it can end up with **too many variables**. The solution is to reduce variables by combining categories that are close to each other, using pivot tables. Naïve Bayes is an exception, because it can handle categorical variables without transforming.

<br>

## Principal Components Analysis

Create new variables that are linear combinations of the original variables. Those linear combinations are **uncorrelated**. Remove the overlap of **information** between variables, and information is measured by **sum of the variances**. 

<br>

{% include lectures/datamining.html %}