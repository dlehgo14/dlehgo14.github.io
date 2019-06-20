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

|          | Calories | Ratings |
| -------- | -------- | ------- |
| Calories | 379.63   | -189.68 |
| Ratings  | -189.63  | 197.32  |

In the above example, the **total variance** is 379.63 + 197.32. Calories accounts for 66%. Below is the graph of it.  

![example](\assets\img\datamining\pca.png)

Z1 and Z2 are two linear combinations, and Z1 has highest, and Z2 has lowest variation. 

```R
cereals.df <- read.csv("02. Cereals.csv")
example <- data.frame(cereals.df$calories, cereals.df$rating)
pcs <- prcomp(example)
pcs
'''
Standard deviations (1, .., p=2):
[1] 22.31646  8.88441

Rotation (n x k) = (2 x 2):
                           PC1       PC2
cereals.df.calories  0.8470535 0.5315077
cereals.df.rating   -0.5315077 0.8470535
'''
summary(pcs)
'''
Importance of components:
                           PC1    PC2
Standard deviation     22.3165 8.8844
Proportion of Variance  0.8632 0.1368
Cumulative Proportion   0.8632 1.0000
'''
```

Z1 and Z2 have correlation 0. (no information overlap) The variables can be more than 2.

```R
pcs <- prcomp(na.omit(cereals.df[,-c(1:3)]))
summary(pcs)
pcs$rotation[,1:5]
'''
                   PC1           PC2           PC3           PC4          PC5
calories  0.0779841812  0.0093115874 -0.6292057595 -0.6010214629  0.454958508
protein  -0.0007567806 -0.0088010282 -0.0010261160  0.0031999095  0.056175970
fat      -0.0001017834 -0.0026991522 -0.0161957859 -0.0252622140 -0.016098458
sodium    0.9802145422 -0.1408957901  0.1359018583 -0.0009680741  0.013948118
fiber    -0.0054127550 -0.0306807512  0.0181910456  0.0204721894  0.013605026
carbo     0.0172462607  0.0167832981 -0.0173699816  0.0259482087  0.349266966
sugars    0.0029888631  0.0002534853 -0.0977049979 -0.1154809105 -0.299066459
potass   -0.1349000039 -0.9865619808 -0.0367824989 -0.0421757390 -0.047150529
vitamins  0.0942933187 -0.0167288404 -0.6919777623  0.7141179984 -0.037008623
shelf    -0.0015414195 -0.0043603994 -0.0124888415  0.0056471836 -0.007876459
weight    0.0005120017 -0.0009992138 -0.0038059565 -0.0025464145  0.003022113
cups      0.0005101111  0.0015910125 -0.0006943214  0.0009853800  0.002148458
rating   -0.0752962922 -0.0717421528  0.3079471212  0.3345338994  0.757708025
'''
```

In this case, sodium is dominant factor. It is because of scale (mg). So **normalization**(=standardization) is needed.

<br>

{% include lectures/datamining.html %}