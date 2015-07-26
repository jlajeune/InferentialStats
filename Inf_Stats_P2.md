---
title: "Confidence Interval Testing of Tooth Growth Data"
author: "Jason LaJeunesse"
date: "July 26, 2015"
output: html_document
---

###Overview

This investigation will look at a set of experimental data for tooth growth in guinea pigs. There were two different supplemental methods and three different dosage levels, and each guinea pig was only given a single dosage level and supplemental method. This analysis will explore confidence intervals to determine if there are significant differences in tooth growth between the different dosage and supplemental method groups.

### Analysis

#### 1.) Load the ToothGrowth data and perform some basic exploratory data analyses 

The first step is to gain some summary information for each of the group of guinea pigs. The first step will be to summarize the data into quantiles.


```r
data(ToothGrowth)

## Get an overall summary table
summary_tgrowth = aggregate(ToothGrowth$len, by=list(ToothGrowth$dose, ToothGrowth$supp), FUN= summary)
summary_tgrowth = cbind.data.frame(summary_tgrowth$Group.1, summary_tgrowth$Group.2, summary_tgrowth$x)
colnames(summary_tgrowth) = c('dose','supp','Min','Q1', 'Median', 'Mean','Q3', 'Max')
```
#### 2.) Provide a basic summary of the data

To provide a basic summary of the data, a box plot is generated with groupings by dosage level and supplemental method. This is an effective way to understand how the data varies based on grouping from a high level.


```r
##Box Plot by supp and dose
par(mfrow = c(1, 1))
boxplot(len~supp*dose, ToothGrowth, main = "Tooth Growth BoxPlot Grouped By Dosage and Supplemental Method")
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2-1.png) 

```r
#Table showing Box Plot Data
summary_tgrowth
```

```
##   dose supp  Min    Q1 Median  Mean    Q3  Max
## 1  0.5   OJ  8.2  9.70  12.25 13.23 16.18 21.5
## 2  1.0   OJ 14.5 20.30  23.45 22.70 25.65 27.3
## 3  2.0   OJ 22.4 24.58  25.95 26.06 27.08 30.9
## 4  0.5   VC  4.2  5.95   7.15  7.98 10.90 11.5
## 5  1.0   VC 13.6 15.27  16.50 16.77 17.30 22.5
## 6  2.0   VC 18.5 23.38  25.95 26.14 28.80 33.9
```

From an initial look, it appears that lengths may vary based on the sample groups.

#### 3.) Use confidence intervals and/or hypothesis tests to compare tooth growth by supp and dose

To investigate further, T-Tests will performed to compare the data when grouped by supplemental methods and then by dosage levels.


```r
#Perform T Test to compare OJ vs VC

oj_sub = subset(ToothGrowth, ToothGrowth$supp == 'OJ')
vc_sub = subset(ToothGrowth, ToothGrowth$supp == 'VC')

t.test(oj_sub$len, vc_sub$len, paired = FALSE)$conf
```

```
## [1] -0.1710156  7.5710156
## attr(,"conf.level")
## [1] 0.95
```
 Most of the range is above zero, giving an indication that a high tooth growth with OJ rather than VC may be likely. However, the 95% confidence interval does include 0 difference on the low end. This indicates that it is possible that there is no difference in means between the two supplemental methods.
 
 Next, T-Tests will be performed to compare each of the dosage level groups.


```r
# Perform T Tests to compare dosages (0.5 vs 1, 0.5 vs 1.5, and 1 vs 1.5)

dos_05_sub = subset(ToothGrowth, ToothGrowth$dose == 0.5)
dos_10_sub = subset(ToothGrowth, ToothGrowth$dose == 1.0)
dos_15_sub = subset(ToothGrowth, ToothGrowth$dose == 2.0)

t.test(dos_05_sub$len, dos_10_sub$len, paired = FALSE)$conf
```

```
## [1] -11.983781  -6.276219
## attr(,"conf.level")
## [1] 0.95
```

```r
t.test(dos_05_sub$len, dos_15_sub$len, paired = FALSE)$conf
```

```
## [1] -18.15617 -12.83383
## attr(,"conf.level")
## [1] 0.95
```

```r
t.test(dos_10_sub$len, dos_15_sub$len, paired = FALSE)$conf
```

```
## [1] -8.996481 -3.733519
## attr(,"conf.level")
## [1] 0.95
```

The T Tests comparing the different dosage sizes all had negative difference 95% Confidence intervals, suggesting that lower dosage
test cases have lower tooth growth on average.

#### 4.) State your conclusions and the assumptions needed for your conclusions

An assumption was made that a T Test would be appropriate for determining the 95% confidence interval of the mean difference. Also, this test assumed that subjects were not paired as each guinea pig was only tested with one dosage level and supplement method.The conclusion is that higher dosages are likely linked to higher tooth growth, and it is possible that the use of OJ versus VC may not have an impact on tooth growth.

