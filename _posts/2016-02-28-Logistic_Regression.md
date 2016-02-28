---
layout: post
tags: machine,learning
date: 2016-02-28 18:30
title: Logistic Regression
published: true
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>
###简介###
Logistic Regression，虽然叫做regression，但是它只是借鉴了回归的思想，主要用于分类问题。
###算法###
Logit函数:$$logit(p)=log(\frac{p}{1-p})=log(p)-log(1-p) $$ 
transforms x to y values along the entire real line.

Inverse-logit函数:$$logit^{-1}=\frac{1}{1+e^{-t}}$$
take x values along the real line and transforms them to y values in range [0,1].

Inverse-logit函数(sigmoid函数)这是一个取值在0到1之间连续函数：

<img src="/assets/image/sigmoid.png" alt="sigmoid" width="300" height="250">

logistic regression模型将正负无穷区间的线性函数\\(\theta^Tx\\)通过inverse-logit函数映射到[0,1]区间，作为y=1的概率。 

假设函数为\\(h\_\theta(x)=\frac{1}{1+e^{-\theta^Tx}}\\)（尝试将这个函数应用到Logit函数，会得到\\(logit(h\_\theta(x))=\theta^Tx\\)），那么：
$$P(y=1|x;\theta) = h\_\theta(x)$$
$$P(y=0|x;\theta) = 1-h\_\theta(x)$$
以上两个式子可以综合写为：
$$p(y|x;\theta) = (h\_\theta(x))^y(1-h\_\theta(x))^{(1-y)}$$

目标函数为最大化概率：
$$ L(\theta)=\prod\_{i=1}^{m}{p(y|x^{(i)};\theta)}
			  =\prod\_{i=1}^{m}{(h\_\theta(x^i))^y{^i}(1-h\_\theta(x^i))^{(1-y^i)}}$$
这个目标函数的求解过程：
首先取对将连乘变为连加
$$l(\theta) = \sum^m\_{i=1}y^{(i)}log(h\_\theta(x^{(i)}))+(1-y^{(i)})log(1-h\_\theta(x^{(i)})$$
然后可以可以采用梯度下降法求最大解:
$$\theta\_j:=\theta\_j+\alpha(y^{i}-h\_\theta(x^{(i)}))x_j^{(i)}$$
 
###实现###
下面是一段采用batch gradient descent实现的求解过程python代码：

```
43 def stoc_gradient_descent(data):
44     feat = ['empty','f1','f2']
45     m = len(data)
46     n = len(feat)
47     alpha = 0.0001
48     data['empty'] = pd.Series(np.ones(m,dtype=np.int16))
49     theta = np.zeros(n,dtype=np.int16)
50     for i,rows in data.iterrows():
51         #plot(data,theta)
52         p = rows['result']-1/(1+math.e**(-theta.dot(rows[feat])))
53         theta = theta+alpha*p*rows[feat]
54         print theta
55     
56     print theta
57     return theta
```
分类的效果如下：

<img src="/assets/image/LR_cla.png" alt='result' width="300" height="300">

也尝试随机梯度下降，但是没有得到很好的结果，原因还在思考过程中。
###应用###
