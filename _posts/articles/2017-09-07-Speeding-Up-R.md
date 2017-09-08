---
layout: post
title: "Ways to Speed Up R"
modified:
categories: articles
excerpt: "Speeding up R computation for simulation or package purposes."
tags: [statistics, SAS]
image: 
feature: 
date: Sep 07, 2017
comments: true
share: true
---

## Replaced all parentheses ( ) by curly brackets { }

1. In [this post](https://radfordneal.wordpress.com/2010/08/15/two-surpising-things-about-r/), author Radford Neal do simple simulation and found there is a big difference between using "()" and "{}" as following: 
```r
> a <- 5; b <- 1; c <- 4
> f <- function (n) for (i in 1:n) d <- 1/{a*{b+c}}
> g <- function (n) for (i in 1:n) d <- 1/(a*(b+c))
> system.time(f(1000000))
 user  system elapsed
 3.92    0.00    3.94
> system.time(g(1000000))
 user  system elapsed
 4.17    0.00    4.17
```

In my own function of robust estimation method, I have to do some simply multiplication and matrix multiplication as well. Sometime thresholding algorithm need to manipulate vectors, matrices and numbers. So some parentheses exist in my function. I follow the suggestion of Radford Neal and replace all parentheses by curly brackets. My function improve a lot. 

```r
# All parentheses:
  user  system elapsed 
  18.036   0.513  20.257
# All brackets
  user  system elapsed  
  16.190   0.475  16.728
```

The elapsed time is surprisingly improved. Based on the defination of these three times, (User time is seconds the computer spent doing calculations. System time is time the operating system core spent responding to the program's requests. Elapsed time is the sum of those two, plus *whatever "waiting around" your program and/or the OS had to do*.) the improvement of curly brackets improve the waiting time of program. Another post about [What caused elapsed time much longer than user time?](https://stackoverflow.com/questions/13688840/what-caused-my-elapsed-time-much-longer-than-user-time) gives me some clues. My case is almost all the time spent was spent in user, not sys(which is only 0.513). That means that my OS is counting the time my program has to wait for data against my program, not against the operating system. In this case, the point that I can improve is decrease the IO frequency.  

## Use "cmpfun" compile function to Byte Code. 

With this in mind, I try simulations to show the differences and an improvement by using ["cmpfun"](https://stat.ethz.ch/R-manual/R-devel/library/compiler/html/compile.html). The improvement is significant and worth trying. 

```r
a <- 1:10000
f <- function (n) {
  for (i in 1:n) 
    b <- sqrt(sum(a^2)) 
  return(b)
  }
g <- function (n) {
  for (i in 1:n) 
    b <- sqrt(sum(as.numeric(a*a))) 
  return(b)}
k <- function (n) {
  for (i in 1:n) 
    b <- F2norm(a) 
  return(b)}
  
# Try cmpfun compiler
TryF <- cmpfun(
  function(n){
    for (i in 1:n) 
      b <- sqrt(sum(a^2))
    return(b)
  })
system.time(T_f <- f(10000))
# user  system elapsed 
# 0.784   0.146   0.941 
system.time(T_g <- g(10000))
# user  system elapsed 
# 1.066   0.181   1.265 
system.time(T_k <- k(10000))
# user  system elapsed 
# 0.769   0.177   0.954 
system.time(T_try <- TryF(10000))
# user  system elapsed 
# 0.565   0.124   0.690 
T_f; T_g; T_k; T_try
# [1] 577393.6
# [1] 577393.6
# [1] 577393.6
# [1] 577393.6
```

Another solution is by [this post](https://www.r-statistics.com/2012/04/speed-up-your-r-code-using-a-just-in-time-jit-compiler/). Tal mentioned that without modification of every function and add cmpfun() on them, you can simply run the following code once, in the beginning of your code. 

```r
require(compiler)
enableJIT(3)
```

A disadvantage is ["we can now turn the JIT back off using “enableJIT(0)”, but it has still already compiled the inner functions (for example fo and slow_func). If we want to un-compile them, we will have to re-create these functions again (run the code that produced them in the first place)."](https://www.r-statistics.com/2012/04/speed-up-your-r-code-using-a-just-in-time-jit-compiler/)

> 理解统计学将有助于更好的理解这个世界的运行与发展！
>
> --Xiaorui (Jeremy) Zhu

> Knowledge is what we know.
> Also, what we know we do not know.
> We discover what we do not know. 
> Essentially by what we know. 
>
> Thus knowledge expands.
>
> With more knowledge we come to know
> More of what we do not know.
> Thus knowledge expands endlessly.
>
> ...
>
> All knowledge is, in final analysis, history.
>
> All sciences are, in the abstract, mathematics.
>
> All judgements are, in their rationale, statistics.
>
> ----C Radhakrishna Rao
>
> "STATISTICS AND TRUTH Putting Chance to Work"

![Smithsonian Image]({{ site.url }}/images/QR.jpeg)
