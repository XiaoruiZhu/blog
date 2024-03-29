---
layout: post
title: Support Vector Machine Notes
modified:
categories: blog
excerpt: All important notes of Support Vector Machine. Include relationship between C, \\( \sigma \\) and underfitting(or overfitting)
tags: [statistics]
image:
feature:
date: 2016-04-18 Mon 13:37
comments: true
share: ture
---

##  Underfitting(overfitting) vs. C

1. Large C (= \\( 1 / \lambda \\), small \\( \lambda \\)) means: Lower bias, Higher variance. **It reveals that the model may be overfitting.** 

2. Small C (= \\( 1 / \lambda \\), large \\( \lambda \\)) means: Higher bias, Lower variance. **It reveals that the model may be underfitting.** 

In other words, if you suspect that the SVM is underfitting dataset, you should try increase C. And decrease C if the SVM is overfitting. 

##  Underfitting(overfitting) vs. \\( \sigma \\)

1. Large \\( \sigma \\): Features \\( f_i \\) vary more smoothly. Higher bias, Lower variance. SVM may be underfitting dataset. 

2. Small \\( \sigma \\): Features \\( f_i \\) vary less smoothly. Lower bias, Higher variance. SVM may be overfitting dataset. 

In other words, if you suspect that the SVM is underfitting dataset, you should try decrease \\( \sigma \\). And increase  \\( \sigma \\) if the SVM is overfitting. 


## Example:  

Suppose you have trained an SVM classifier with a Gaussian kernel, and it learned the following decision boundary on the training set:

![Smithsonian Image]({{ site.url }}/images/svm-quiz1.jpg)

You suspect that the SVM is underfitting your dataset. Should you try increasing or decreasing C? Increasing or decreasing σ2?

**It would be reasonable to try increasing C. It would also be reasonable to try decreasing σ2.**

## features(n) with sample size(m)

1. If n is large (relative to m): (n=10,000, m= 10~1000)
    Use logistic regression, or SVM without a kernal.

2. If n is samll, m is intermediate(10~10,000):
    Use SVM with Gaussian Kernal.

3. If n is small(1~1000), m is large(50,000 or more), 
    Add more features, then use logistic regression or SVM without kernal. 

** In most of these situations above, Neural network likely to work well, but the computating bottleneck is the issue for big dataset.**

## Summary

> Support Vector Machine can be used to ... many classification problems. 
>
> -- Jeremy Zhu

*Please contact before you try to reprint. Wechat account: “IamEveryone"*

![Smithsonian Image]({{ site.url }}/images/tips.JPG)

![Smithsonian Image]({{ site.url }}/images/QR.jpeg)
