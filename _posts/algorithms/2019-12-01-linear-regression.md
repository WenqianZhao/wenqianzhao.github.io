---
layout: post
title: "「机器学习基础系列」：线性回归"
subtitle: "Linear Regression"
author: "Wenqian"
header-mask: 0.4
tags:
  - 机器学习
  - 回归
---

## 线性回归的形式
线性回归即由一个线性方程来拟合目标值。其形式如下：

\mathrm{y}(\boldsymbol{\mathrm{x}},\boldsymbol{\mathrm{w}})=w_0 + w_1x_1 + ... + w_Dx_D
其中\boldsymbol{\mathrm{x}}=(x_1,...,x_D)^T。


## 平方损失函数的概率解释



假定目标值t通过一个确定性方程y(x,w)以及一个高斯噪声
