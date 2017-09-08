---
layout: post
title: R is extremely strong. 不得不说R如此强大！
modified: 2017-04-29 Sat 00:18
categories: articles
excerpt: I have to say that R is extremely strong. 不得不说R如此强大！
tags: [R, Statistics, Machine Learning, emacs]
comments: true
share: true
image:
feature:
date: July 09, 2013
---

## R强大之处的几点表现。

1. 可以灵活的矩阵，向量运算，这在SAS里是不可想象的，具体资料google即可。

2. 在R中用包来省去非常浩大的工作！这里我要强烈推荐包的作用，为什么：

 - 首先，很多时候，你不需要像C语言码农一样去写一大堆的循环，去编算法实现MCMC，去编程得到多参数模型的参数估计值。这些对一个高端C码农来说我觉得都有些困难，不是黑计算机专业同学，只是这些后面有一大堆的统计学知识需要非常熟悉，不只是知道怎么用，还要熟悉其理论原理，这样才能顺畅的编写出函数（写过各种统计方法的函数的同学应该懂的）。
 
 - 其次，就算你真正对统计原理很熟悉，那你还真不一定花了很多时间在编程语言上，至少在做统计分析时，C应该不是使用频率最高的。你或许在SAS里面游走，处理dataset，用一用Proc，这些已经足够你的使用，你没有机会了解到Proc背后都是些什么计算过程。亦或是在Python里面直接使用丰富的数据分析包。或对中国社会科学类专业，就直接还在用SPSS，（学统计的常常爱黑SPSS，在这里就不黑了，下文有一节我要说一说有些统计人不好的偏见），那么就更不谈了，点点鼠标，熟悉各种统计方法的应用就好了。但是当你要解决一个新的问题需要改变计算过程，改变具体计算过程、算法，或是要写论文做模拟。。。那么你需要的一定是R，或者至少是MATLAB or Python.
 
 - 最后，强大自由度，说了这么多废话打住。

3. R的包一直在更新，你可以找到基础的stat,timeseries,lm,logit等等的包，应用尽用，只要知道你要解决什么问题，这个问题属于哪个统计范畴，统计术语英文叫什么，花点儿时间google或者看看SCRN应该没有问题。找到包之后下载pdf介绍文件仔细读一读应该就能解决你的问题，接下来就着手编写程序解决具体问题吧。当分析结果到手，如何解释，如果提出给出合理的原因也是一个重要的方面。

 - HOMEPAGE 可以从上面下载到包，也可以读包的介绍文档  [[http://www.r-project.org/][R homepage]]
 
 - 但是我还是要推荐一个IDE: [[www.rstudio.com][R-studio]] 会让你下载包快非常多，管理文档，包也会方便很多还能创建project编写较大一点儿的项目或者是包。

4. 那些精美开发包类似lm, glm, mgcv, lme4 等等就不再赞美啦。另外强调一点：不是所有的包结果都是对的！用多了你就懂了，多看看他的解释文档，没写清楚的话读读源代码，直接R中运行函数名就出来了。很多时候信息是不对称的，写包的人可能与你用的统计理论是不一样的，所以算出来的结果和你想的天差地别，实在不行就自己编吧。这就是开源的一个潜在缺陷吧，质量层次不齐。

## 裂墙推荐的包

1. tseries; xts. 网络数据抓取。做实证分析必备，不过主要是金融数据，宏观数据自己到数据库或者统计局网站下载去吧。

{% highlight r %}
##----------数据下载载入包---------
library(tseries)
library(xts)
## ------再写个函数-------
getdata=function(quote,start,savename){
  quote=as.xts(get.hist.quote(quote,start=start,
                            provider ="yahoo",
                            quote=c("Open", "High", "Low", "Close","Volume","AdjClose")))
  save=paste("Y:/DATA/",savename,start,".csv",sep='')
  write.csv(quote,file=save)
  return(invisible(quote))
}
##---------------挑选各个股指-----------
##9I0001  ^DJI	Dow Jones Averages(30 Industrials) 道琼斯30指数
getdata(quote="^DJI",start="2013-01-02",savename="DJI")
{% endhighlight %}

2. nlminb, optim, optimized等函数，不是包，但是对于参数估计有帮助，需要更多信息网上查去吧，后续我会写一点。

3. Sweave函数实现保存数据分析结果。这个我会转载一篇文章，对于写论文的同学有福了。

4. 这两天一边用Emacs写论文，一遍在跑R的代码，用进程跑大型模拟确实避免了崩溃的局面。曾经尝试用R-studio跑十几段分别需要模拟1000次的函数，在我输入到第四五段的时候一般R-studio就崩溃了，我还加入了跑完一段清空内存的命令呢照样崩溃。也不赖他，太大了，每段都得2个小时左右啊。 

 - 强推开始了“并行计算”当当当当。来源于一片文章[[http://xccds1977.blogspot.com/2012/09/parallelforeach.html][用Parallel和foreach包玩转并行计算]]，有兴趣的可以仔细看看来实现，不难，我测试过博主给的实例，但是有一点需要提出，伤电脑！四核同时开跑我的电脑确实吃不消，不一会儿鲁大师开始报警温度过高，所以用自己电脑我觉得还是踏实单核用Emacs调用进程扔到夜里踏踏实实跑。不用R-studio原因是有可能半夜崩溃了你早上起来发现论文或者模拟计划又要往后拖一天。

----------

暂时写到这2013/7/9 17:53

## 新的感悟

### R Studio 的骨灰级功能：活在R Studio里
