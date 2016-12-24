---
layout: post
title: Simulation of Evalution Matrics in Machine Learning
categories: blog, Statistics, R, Machine Learning
excerpt: Simulation of Evalution Matrics in Machine Learning
tags: [R, Statistics, Machine Learning]
comments: true
share: true
image:
feature:
date: `r format(Sys.time(), "%d %B, %Y")`
---

This post is the simulation work for many different evaluation matrics in Machine Learning filed. In order to show what matrics work in certain situations, for different setting (including balanced data, imbalanced data), this study shows performance of different matrics so that the applicabilities of each one can be seen clearly. 

# Evaluation Matrics

In machine learning filed, various different performance evaluation matrics are available for the selection of models and algorithms. Having a lot of tools in your hand never means a bad news. The potential problems, however, include which one is the best for my situation, why use this one, is this one robust for outliers or imbalance. Even the matrics is good enough, does the one you choicen have good performance when the data size increase? These question inspire me to do these simulations in this post. 

On the other hand, this post will be helpful for someone who want to learn and implement these matrics in his or her projects. Simultaneously, better understaning leads better choices and decisions. So let's start. 

## AUC-ROC (Area under receiver operating characteristic curve)

It's easy and may be one of the most popular matric among academics. In pratical, it seems not that popular. It will show the performance of the binary classifier by changing threshold probability. 

## AUC-PR (Area under precision recall curve)

## Hosmer-Lemeshow test

## Decile table

## 

```
library(quantmod)
library(ggplot2)
getSymbols('^SSEC',src='yahoo',from = '1997-01-01')
close <- (Cl(SSEC))
time <- index(close)
value <- as.vector(close)
p <- ggplot(data.frame(time,value),aes(time,value))
p + geom_line()
```

原文作者展示了如何在图中加入一些其它的说明元素来丰富视图中所包含的信息。通过两种颜色来表示“江核心”和“胡核心”的执政时期，以及在图中标注若干重大事件。代码如下。

```
yrng <- range(value)
xrng <- range(time)
data <- data.frame(start=as.Date(c('1997-01-01','2003-01-01')),end=as.Date(c('2002-12-30','2012-01-20')),core=c('jiang','hu'))
timepoint <- as.Date(c('1999-07-02','2001-07-26','2005-04-29','2008-01-10','2010-03-31'))
events <- c('证券法实施','国有股减持','股权分置改革','次贷危机爆发','融资融券试点')
data2 <- data.frame(timepoint,events,stock=value[time %in% timepoint])
p + geom_line()
  + geom_rect(aes(NULL,NULL,xmin = start, xmax = end, fill = core),ymin = yrng[1],ymax=yrng[2],data = data)
  + scale_fill_manual(values = alpha(c('blue','red'),0.2))
  + geom_text(aes(timepoint, stock, label = events),data = data2,vjust = -2,size = 5)
  + geom_point(aes(timepoint, stock),data = data2,size = 5,colour = alpha('red',0.5))
```

借鉴于： http://xccds1977.blogspot.com/2012/01/ggplot2_29.html