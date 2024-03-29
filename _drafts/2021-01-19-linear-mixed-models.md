---
title: 线性混合模型
tags:
---
## 线性混合模型的定义
线性混合模型是一种常用线性模型，它拟合两种效应，即随机效应（random effects）和固定效应（fixed effects）。
一般而言固定效应拟合的是我们感兴趣的方差分量，例如，性别和处理条件等。
而随机效应拟合的是我们不感兴趣但又不可忽视的方差分量，例如，TODO

## 线性混合模型能解决什么问题
线性模型是最常见的回归模型之一，构建该类模型的重要条件是每个样本之间是相互独立的。
当每个样本之间不相互独立时，例如技术重复。
对于每个个体而言，在重复测量后所得的因变量，彼此间并非相互独立。
此时我们需要把方差分割为个体内部重复测量的方差分量和个体之间的方差分量。

## 随机效应
在此我们用一个实例来解释如何使用线性混合模型来解决上述问题。
该实例是一个关于政治家声音频率的差别，在这个例子中我们感兴趣的是不同人之间的差别。
然而为了使研究更准确，每个人的声音频率被记录了多次。
这意味着每个样本之间是不完全独立的，而就一个人而言，其声音频率是服从一定分布的，比如正态分布。
我们关注的是这些分布的差异，即这些分布的均值的差异，而这些均值就是最能代表每个人的参数。

下面我们对数据做初步探索。
首先我们可以知道一共有6个研究对象，每个研究对象记录了两个不同的状态，每个状态下有7次测量结果。
```

```

下面通过绘制箱线图，对我们的数据做初步的认识。
```

```
![boxplot-per-subject.png](../images/linear-mixed-models-boxplot-per-subject.png)

## 用随机截距模拟个人的平均值


```

```



## 参考文献 
1. [Section Week 8 - Linear Mixed Models](https://web.stanford.edu/class/psych252/section/Mixed_models_tutorial.html)
