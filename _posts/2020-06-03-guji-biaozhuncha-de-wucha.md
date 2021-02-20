---
layout: post
title:  估计标准差的误差
author: maiming
date:   2020-06-03
categories: analysis
tags: 统计学 误差
description: 标准差跟平均身高一样，存在随机误差，如果样本量不大，那么这个随机误差还不小。
license: 'cc-by-3.0-cn'
enable_mathjax: true
---

为了简单起见，假设身高服从某个未知的正态分布$\mathcal N\left(m, \sigma^2\right)$。我们从中抽出$N$个样本$x_1, \ldots, x_N$，可以计算得到（无偏）的标准差是
\\[ \hat \sigma = \sqrt{\frac{\sum_{i=1}^N \left(x_i - \overline x\right)^2}{N-1}} \\]
其中\\(\overline x = \left(x_1 + \cdots + x_N\right)/N\\)是样本的均值。

也就是说\\(\left(N-1\right){\hat \sigma}^2\\)遵循\\(N-1\\)自由度的卡方分布的\\(\sigma^2\\)倍，也就是\\(\sigma^2\chi^2\left(N-1\right)\\)。我们知道\\(\chi^2\left(N-1\right)\\)可以认为是\\(N-1\\)个独立正态的平方和，所以可以写出
\\[\left(N-1\right) \frac{ {\hat \sigma}^2 }{\sigma^2} \sim \mathcal{N}\_1^2 + \cdots + \mathcal{N}\_{N-1}^2 \\]
在\\(N\\)比较大的时候，可以使用中心极限定理进行近似
\\[ \sqrt{ \frac{N-1}{2} } \left( \frac{ {\hat \sigma}^2 }{\sigma^2} - 1\right) \sim_{近似} \mathcal N \left(0,1\right) \\]

按照正态分布，以95.4%的概率，下式成立
\\[ -2 \leq \sqrt{ \frac{N-1}{2} } \left( \frac{ {\hat \sigma}^2 }{\sigma^2} - 1\right) \leq 2 \\]
也就是说以95.4%的概率
\\[ \sigma \in \left[  \frac{ {\hat \sigma} }{ \sqrt{1 + \sqrt{ \frac{2}{ N-1 } } } } , \frac{ {\hat \sigma} }{ \sqrt{1 - \sqrt{ \frac{2}{ N-1 } } } } \right] \\]
在\\(N\\)足够大的时候，这个公式可以简化为
\\[ \sigma \in \left[ \left( 1 - \sqrt{ \frac{1}{2 \left(N-1\right)} }\right) \hat \sigma, \left( 1 + \sqrt{ \frac{1}{2 \left(N-1\right)} }\right) \hat \sigma \right] \\]

## 一个例子

已知2014年学生体质与健康调研中，北京城男17、18岁的标准差为5.83、6.06，样本量均为150，那么代入上述公式，可以知道，这两个年龄段的标准差95%置信区间是\\( \left[ 5.49, 6.16 \right] \\)、\\( \left[ 5.71, 6.41 \right] \\)。横跨的标准差基本上在0.6厘米了。用如此不准的标准差，去推算高个子比例，得到的结论自然是不靠谱的。