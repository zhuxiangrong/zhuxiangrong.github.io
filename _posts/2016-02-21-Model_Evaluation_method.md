---
layout: post
tags: machine,learning
date: 2016-02-21 18:30
title: Model Evaluation Methods
published: true
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>
###简介###
这篇文章结合一个实际案例总结一下常用的二分类模型效果评价方法。
####case data####
随机森林产生的6651个样本的分类结果,其中有1848个正样本，4803个负样本。
####confusion matrix####
<img src="/assets/image/confusion_matrix.png" alt="confusion_matrix" width="300" height="250">

基于上面的四个定义，有下面的评价指标：
* 正确率,Accuracy : TP+TN / TP+FP+TN+FN,0.9703
* 错误率,Error rate: 1-Accuracy,0.0296
* 灵敏性,Sensitivity,又叫击中率或真正率：TP/TP+FN,0.9779
* 特效性,Specificity,又叫真负率,TN/TN+FP,0.9677
* 精度,Precision:TP/TP+FP,0.9139
* 假正率,负样本中被识别为正的比率,False Positive Rate:FP/TN+FP,0.0322
* 负正元率,被识别为负的样本中真负样本的比例,Negative Predictive Value:TN/TN+FN,0.9920
* 正元错误率,被识别为正样本中错分的,False Decovery Rate:FP/TP+FP,0.0860
具体用哪个指标评价，需要根据实际业务需求来。
####Roc曲线####
Roc曲线的横轴是假正率，即负样本中被错误的判断为正样本的比率；纵轴是真正率，即正样本被正确判断的比率。真正率的增加是以假正率的正价为代价的。ROC曲线下方的面积被叫做AUC，为比较模型准确性的指标，AUC越大模型效果越好，即同样的假正率下好的模型真正率更高。
ROC曲线的自作过程：将样本按照概率从高向低排序，从高到低变化阈值来确定样本是1或者和0，得到不同阈值下的假正率和真正率，将其画到坐标系中，最终形成ROC曲线。
python代码如下：

```
import numpy as np
ts = np.concatenate((np.array([1]),data['probability1'].values,np.array([0]))).tolist()
sorted(ts,reverse=True)
x = []
y = []
for t in ts:
    above_p = len(data[(data['probability1']>t) &(data['label']==1)])
    above_n = len(data[(data['probability1']>t) &(data['label']==0)])
    x.append(above_n/float(Ne))
    y.append(above_p/float(P))    
rocdf = pd.DataFrame({'false_positive':x,'true_positve':y})
rocdf=rocdf.sort('false_positive')
plt.plot(rocdf['false_positive'],rocdf['true_positve'])
plt.xlabel('False Positive Rate')
plt.ylabel('Sensitivity')
plt.title('ROC')
plt.xlim(-0.1,1)
plt.ylim(0,1.1)
plt.show()
```
ROC曲线如下：

<img src="/assets/image/roc.png" alt="roc" width="300" height="300">
####KS值####
KS值表示模型将正负样本分开的程度，是一种常用的判断二元分类模型准确度的方法，通常KS大于0.2就表示模型有比较好的预测准确性了。
KS值曲线的绘制方法：（1）将该概率分区间 （2）计算每个区间内真实正负样本的累计占比（3）横轴为区间，纵轴为正样本累计比例和负样本累计比例

####Lift值####
Lift是实际应用中使用很多的一种评价方法，它通俗易懂，即是用来评价使用模型比不使用模型效果提升的程度，举个例子，一个人群的平均响应率是5%，如果某个模型划分出一部分人能够得到20%的响应率，那么这个模型的lift为20%/5%=4.0。

#### F-Score####










