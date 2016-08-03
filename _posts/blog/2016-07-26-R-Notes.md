---
layout: post
title: R learning Notes
modified: 2016-08-03 Wed 17:46
categories: blog
excerpt: R ggplot2 包的时间序列处理
tags: [R, ggplot2]
comments: true
share: true
image:
feature:
date: 2016-07-26 Tue 22:38
---

ggplot2包对于时间序列数据的绘图：

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