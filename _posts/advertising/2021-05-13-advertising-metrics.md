---
layout: post
title: "广告领域常用分析指标"
subtitle: ""
author: "Wenqian"
header-mask: 0.4
mathjax: true
tags:
  - 广告算法
  - 实时竞价
---

## 常用分析指标
### 业务指标
1 - GMV（Gross Merchandise Volume）

网站成交金额。在广告领域，我们一般更关心**广告GMV**，即广告所带来的GMV。各大电商广告平台一般将广告GMV作为最主要的优化目标。

2 - ROI

投资回报率，或者叫投入产出比。计算公式为：广告带来的销售收入 / 投放广告开销 * 100%。ROI是一个对于广告主来说非常重要的指标，广告平台一般在优化GMV时会将广告主的ROI作为多个限制条件中的一个。

3 - 广告收入

即广告平台的收入，也是所有广告主的广告开销之和。值得注意的是，虽然广告收入很重要，但也不是一定越多越好，因为过分追求广告收入可能会降低广告主的ROI，并且损害用户（看广告的人）的体验。

4 - CTR & CVR

CTR表示点击率，而CVR表示转化率。转化一般指购买行为，也可以定义为收藏、询店等其他关心的行为。实时广告一般以点击计费，而像OCPC这种方式的广告出价会直接和预估CVR挂钩，因此CTR和CVR（包括建模对其进行预估）都是十分重要的。

5 - COPC

计算公式为 copc = 平均真实CVR / 平均预测CVR。这个指标一般用来看预测的CVR和真实值之间的偏差，和1越接近越好。（注：也有把CVR换成CTR的）

6 - 溢出率

计算公式为 溢出率 = 广告主投放广告开销 / （广告主期望出价 * 订单数（或点击数））。由于广告主一般只是给出（一个点击或一个转化下的）期望出价以及出价范围，而不会直接参与每次曝光的竞价，广告平台会根据不同策略对出价进行调整，因此广告主投放广告的实际总开销和其期望开销可能存在一定差距。广告主肯定是希望实际开销低一些，但是这样相对于平台被“白嫖”，因此一般来说都是和1越接近越好。不同场景下的要求可能不同。

7 - 笔单价和客单价

即平均每笔订单价格以及平均每个用户的消费价格。这个有可能会看一下。


### 模型指标
1 - AUC和gAUC

AUC即ROC曲线下的面积，用来衡量模型对待分类问题的一个排序能力。gAUC则是group的AUC，可以理解为对各个group分别计算分类效果，然后再进行加权求和。AUC除了可以通过计算ROC曲线下的面积得到，也可以被看做是任意取两个正负样本，正样本的预估点击率大于负样本的概率。

2 - logloss

对数损失，一般用于分类问题，例如CTR和CVR预估中。