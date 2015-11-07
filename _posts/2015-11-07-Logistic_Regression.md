---
layout: post
tags: machine,learning
date: 2015-11-07 18:30
title: Logistic Regression
published: true
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>
## Logistic Regression ##
###简介###
Logistic Regression，虽然叫做regression，但是它只是借鉴了回归的思想，主要用于分类问题。
###算法###
Logistic Regression按照linear regression的方式做分类。要拟合的值是离散的0和1，所以用sigmoid函数将值域限制在0-1之间。sigmoid函数公式：
$$ g(z)=\frac{1}{1+e^{-z}}$$ 这是一个取值在0到1之间连续函数：

![sigmoid](../assets/image/sigmoid.png =250x250)

logistic regress的假设函数为
\\( h_\theta(x)=\frac{1}{1+e^{-\theta^Tx}} \\),并把它看作是y=1的概率。的确，x偏离平面\\\(\theta^Tx=0\\)越远，\\( h\_\theta(x)\\)越接近1或者0。因此假设：
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
$$\theta_j:=\theta_j+\alpha(y^{i}-h\_\theta(x^{(i)}))x_j^{(i)}$$
 
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

![sigmoid](../assets/image/LR_cla.png =300x300)

也尝试随机梯度下降，但是没有得到很好的结果，原因还在思考过程中。
###应用###
