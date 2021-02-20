---

layout: post
title: 估计身高测量中的随机误差
author: maiming
date: 2020-04-24
categories: analysis
tags: 统计学 误差
description: 究竟有多不准？
discuss_link: https://tieba.baidu.com/p/6635675829
enable_mathjax: true
enable_comments: true
license: 'cc-by-3.0-cn'

---

<div style="display:none">
  $$
    \newcommand{\Cov}{\mathop{\rm Cov}\nolimits}
    \newcommand{\Var}{\mathop{\rm Var}\nolimits}
    \newcommand{\E}{\mathop{\mathbb E}\nolimits}
  $$
</div>


假设我们测量某个年龄段（$x \sim x + 1$岁）某性别的身高，建立下面的统计模型。

## 模型

假设年龄$a$遵循$[x, x + 1]$中的某个未知分布，
给定年龄情况下，身高遵循正态分布。假设$f, s$是标准生长曲线的中位值和标准差，那么这个样本的身高是

$$h = f(a) + s(a) \mathcal N$$

其中$\mathcal N$是独立分布的标准正态变量。

身高$h$的条件期望是$\E[h\|a] = f(a)$，条件方差是$\Var[h\|a] = s(a)$。

而$h$的总体均值和方差满足

<div>
  \begin{align}
  \E a &= x + \frac 12 \\

  \Var a &\leq \frac 14 \\

  \E h &= \E f(a) \\

  \Var h 
  &= \E\left[h^2 \right] - \left(\E h\right)^2 \\
  &= \E\left[\E\left[h^2\vert a\right]\right] - \left(\E f(a)\right)^2 \\
  &= \E\left(f(a)^2 + s(a)^2\right) - \left(\E f(a)\right)^2 \\
  &= \Var f(a) + \E\left[s(a)^2 \right] \\

  \Cov(a, h) 
  &= \E[ha] - \E[h]\E[a] \\
  &= \E[f(a)a] - \E[f(a)]\E a
  \end{align}
</div>

## 代入数据

现在代入实际数据计算。使用中国2005生长表中男性生长最快的12～13岁。

<table>
  <thead>
    <tr>
      <td>年龄/岁</td>
      <td>身高中位数/cm</td>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>12.0</td>
      <td>151.9</td>
    </tr>
    <tr>
      <td>12.5</td>
      <td>155.6</td>
    </tr>
    <tr>
      <td>13.0</td>
      <td>159.5</td>
    </tr>
  </tbody>
  <caption>
    中国2005年生长发育标准中12.0～13.0岁身高中位数
  </caption>
</table>

发现此段内如用线性函数替代，误差不超过0.1厘米，这是我们能够接受的。假设$f(a) = ka + b$，均值和方差简化为

$$\E a = 12.5$$

$$\E h = f(\E a) = f(12.5)$$

$$\Var h = k^2 \Var a + \E\left[s(a)^2\right]$$

$$\Cov(a, h) = k \Var a $$

## 误差来源分析

$\Var h$的第一项可认为是年龄「波动」造成的误差，满足
\\[k^2 \Var a \leq \frac{k^2}{4} = 14.44\\]

如果认为$a$满足$[x,x+1]$上的均匀分布，那么$\Var a = \frac 1{12}$，$k^2 \Var a = 4.81$。

第二项$\E\left[s(a)^2\right]$代表了人群「内生」的身高变异，查询生长表，可以得到估算
\\[50 \leq \E\left[s(a)^2\right] \leq 65\\]
也就是说$\E\left[s(a)^2\right] \gg k^2 \Var a$。

## 结论

身高测量的随机误差$\Var h$主要是由**内生的身高变异**，而不是测量年龄的偏差带来的。

我们还计算了（协）方差$\Var a$，$\Cov(a, h)$，在估计身高的偏差时候暂时用不到，但是会在拟合生长曲线的时候有用。