---
layout: post
title: "「机器学习中的高斯过程」：高斯过程回归"
subtitle: "Gaussian Process Regression"
author: "Wenqian"
header-mask: 0.4
mathjax: true
tags:
  - 机器学习
  - 高斯过程
---

## 什么是高斯过程回归
在上一篇文章中，我们讲到高斯过程方法可以用来解决有监督机器学习中的回归问题和分类问题，那么高斯过程回归其实就是指用高斯过程来解决回归问题。

## 高斯过程回归模型的两种解释
我们可以从两种角度来看待高斯过程回归模型。一种是**将一个高斯过程看做是一个关于方程的分布，并且推断（inference）直接作用于方程空间**，这种角度被称为 *function-space view（方程空间视角）* 。另一种视角相对熟悉并且更容易被理解，被称作 *weight-space view（权重空间视角）* ，我们将在下面先对其进行介绍。

### 权重空间视角
在将高斯过程回归之前，我们先看看标准的线性回归是什么样的。考虑一个带有高斯噪声的标准线性回归模型：

$$f\(x\) = \textbf\{x\}^T\textbf\{w\},\space\space\space\space y = f(\textbf\{x\})+\epsilon\space,$$

其中 $\textbf{x}$ 是输入向量， $\textbf{w}$ 是线性模型的权重，$f$ 是模型方程，而 $y$ 是观察到的标签。 $\epsilon$ 是一个均值为0且方差为 $\sigma^{2}_{n}$ 的随机变量：

$$\epsilon ~ \mathcal{N}\(0, \sigma^{2}_{n}\)$$

标签带有高斯噪声是线性回归中的一个通常的假设，由此我们可以得到训练集上的似然函数为：

![img](/img/in-post/ml/gp/gp2-1.png)

这里很巧妙的利用了欧式距离（本质上是平方和）以及指数函数指数部分相乘等于相加的关系，将似然函数转化为了关于矩阵的一个高斯分布。这时如果我们假设 $\textbf{w}$ 的分布为 $\textbf{w} \backsim \mathcal{N\(\textbf{0}, \Sigma_p\)}$ ，那么根据贝叶斯定理，就可以得到关于 $\textbf{w}$的后验概率为：

![img](/img/in-post/ml/gp/gp2-2.png)

其中：

![img](/img/in-post/ml/gp/gp2-3.png)

如果令 $\mathnormal\{A\} = \sigma^\{-2\}_\{n\}\mathnormal\{X\}\mathnormal\{X\}_\{T\} + \Sigma^\{-1\}_\{p\}$ ，那么我们可以进一步地得到 $\textbf{w}$ 的后验概率为：

![img](/img/in-post/ml/gp/gp2-4.png)

也就是说同样是一个高斯分布。如果我们将上述 $\textbf{w}$ 的后验概率作为权重，并针对 $\textbf{w}$ 做积分，就可以得到同样是高斯分布的预测方程分布：

![img](/img/in-post/ml/gp/gp2-5.png)

### 方程空间视角
以方程空间的视角来看，我们用一个高斯过程（GP）来描述一个关于方程的分布。在《Gaussian Processes for Machine Learning》中，作者对高斯过程的定义是“一个随机变量的集合，其中任意有限个随机变量满足联合高斯分布”[注1]。

> 注1：英文原文为“A Gaussian process is a collection of random variables, any finite number of which have a joint Gaussian distribution”。

一个高斯分布完全由其均值方程和协方差方程决定。我们定义一个真实过程 $\mathnormal\{f\}\(\textbf{x}\)$ 的均值方程和协方差方程为：

![img](/img/in-post/ml/gp/gp2-6.png)

那么高斯过程则可以被表示成：

![img](/img/in-post/ml/gp/gp2-7.png)

通常，我们会将均值方程设置为0方程（即方程结果恒为0）。

未完待续
