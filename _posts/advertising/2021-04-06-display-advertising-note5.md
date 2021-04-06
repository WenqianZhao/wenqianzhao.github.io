---
layout: post
title: "Display Advertising with
Real-Time Bidding (RTB) and
Behavioural Targeting读书笔记（五）"
subtitle: "归因模型"
author: "Wenqian"
header-mask: 0.4
mathjax: true
tags:
  - 机器学习
  - 广告算法
  - 实时竞价
---
一个用户的最终转化（conversion）通常是多个广告事件一起贡献的结果，这些事件我们称作触点（touchpoints）。正如下图所示，转化归因其实就是一个分数分配问题，看看各个通道对于最终转换的驱动是多少。

![img](/img/in-post/advertising/conversion-attribution.png)

获得一个“正确的”归因模型的很重要的，因为我们需要它来帮助我们在不同的通道或活动中重新分配预算。不过这个问题从理论和实践角度都是很难解决的，因为不存在“ground-truth”数据（即我们无法知道分数到底应该如何分配给各个通道）。但它同时又很重要，因为可以有效防止广告系统中的作弊与博弈（这里我理解为是不公平博弈）行为发生。

## 启发式模型
下面给出几个在Google Analytics里提到的启发式模型：

1. last touch：最后一个触点获得100%的分数。**Last Touch归因（LTA）模型是现在在展示广告中用的最广的归因模型**。LTA的问题在于鼓励DSP取专注于最后一个触点，比如那些重定向用户（retargeted users，也就是已经展露出对广告背后商品的兴趣的用户），的活动预算，这样可能会损失部分新用户，从而影响广告主的长期利益。
2. First touch：第一个触点获得100%的分数。这个比较**适合拉新**，其优势和问题和上面正好相反。
3. Linear touch：每个触点获得相同分数。比较适合那种设计用来保证用户留存的广告活动。
4. Position based：更关注第一个以及最后一个触点，适合鼓励传播以及以效果为导向的活动。
5. Time decay：新的触点会被认为比老的触点更有贡献。比较适合短期业务。
6. Customised：广告主自定义的归因方式。

![img](/img/in-post/advertising/heuristic.png)

上面是一个更直观的图解。很显然，**启发式模型远没有达到最优**。一个参与方可以很轻易地掌控整个系统，特别是在采用last touch和first touch归因模型的地方。接下来我们将介绍一下多触（multi-touch）的以及数据驱动的归因模型。

## 数据驱动概率模型
在介绍数据驱动概率模型之前先简单说一下**Shapley值**。Shapley值是博弈论中的一个概念，用于在玩家联盟（coalition）中公平分配整个游戏（game）的效用（utility）。在在线广告的场景下，第 $k$ 个触点的Shapley值 $V_k$ 为：

![img](/img/in-post/advertising/shapley.png)

其中 $C$ 是所有通道的集合；$S$ 是 $C\\k$ 内的一个任意子集，包括空集；$y$ 表示联合游戏中获取的效用。**由此我们可以得到 $k$ 的Shapley值是 $y$ 的加权平均期望增益，其中权重是一个长度为 $\|C\|$ 的序列以 $S,k$ 为起始的概率**。可以看出，期望这块是通过统计数据计算得到的，但**权重和数据本身无关**。那数据驱动概率模型又是什么呢？

数据驱动模型是Shao和Li等人提出的。在他们的论文中实现了两种模型，一个是Bagged逻辑回归，另一个是简单的概率模型。其中Bagged逻辑回归是用于预测用户在给定当前广告接触（touch）事件后是否会完成转化。逻辑回归的输入数据是用户的广告接触事件 $x = [x_1,x_2,...,x_n]$ ，其中每一项是0或1，表示用户是否被对应通道touch。

简单概率模型利用了一阶和二阶通道条件转化概率，其中一阶条件转化概率为：

![img](/img/in-post/advertising/first-order.png)

其中 $N_{positive}(x_{i})$ 和 $N_{negative}(x_{i})$ 分别表示暴露给通道 $i$ 且最终完成或未完成转化的用户数量。类似的，二阶通道条件转化概率为：

![img](/img/in-post/advertising/second-order.png)

由此可以得到通道 $i$ 的贡献为：

![img](/img/in-post/advertising/channel-sum.png)

其中 $N_{j{\neq}i}$ 表示不为 $i$ 的通道数量。从上面我们可以看出这个概率模型**其实就是Shapley值模型的一个简化版**：（1）只考虑了一阶和二阶通道贡献；（2）一阶条将来的权重直接被设置为1/2，有些武断了。

## 其他模型
其他模型包括：基于一个叫转化漏斗（conversion funnel）的概念，利用隐马尔可夫模型来解决归因问题的；给出马尔科夫图来给用户行为中的一阶和高阶马尔科夫行走进行建模的；还有其他一些模型就不一一给出了，有兴趣的朋友可以找到书本的对应章节继续深入阅读。

## 归因模型的应用
### 基于提升的竞价（Lift-based bidding）
传统的竞价策略被称为基于价值（value-based）的竞价策略，因为这些方法都是基于潜在曝光的期望价值（也就是utility）的。而基于提升的竞价策略则是将报价看做是转化价值 $r$ 和用户在看到该广告后转化率的提升 $\Delta\theta$ 的乘积：

![img](/img/in-post/advertising/blift.png)

提升的CVR其实就是一个multi-touch归因模型中当前触点所分配的转化分数：

![img](/img/in-post/advertising/blift2.png)

**基于提升的竞价策略的基础假设是用户可以在任何上下文中完成转化**，即便根本没有任何广告曝光。因此在任意上下文 $H$ 中（假设已经有了一些touch），我们可以得到每个用户的基本转化率 $\theta = P(conv\|H)$ ，那么某个广告曝光可能带来的用户转化率提升就是 $\Delta\theta = P(conv\|H,h) - P(conv\|H)$ 。Xu等人提出用GBDT来估计 $P(conv\|H)$ ，并以此计算CVR提升。依据他们的实验结果，这种方法相比基于价值的竞价策略可以带来更多转化。然而实际上更多的转换被算在了更具竞争性的基于价值的竞价策略上，因为采用了last-touch的归因机制。所以说只有采用了multi-touch归因机制的RTB市场才能真正推广基于提升的竞价策略。

### 预算分配
假设一个活动有 $n$ 个通道： $X = x_1,x_2,...,x_n$ ，且每个通道 $x_i$ 的最大花销可以是 $S_i$ ，活动的总预算是 $B$ ，并且每个通道的ROI是 $R_i$ ，那么预算分配问题可以被表示为：

![img](/img/in-post/advertising/budget.png)

其中 $B_i$ 表示每个通道被分配的预算，也是我们要进行优化的。 $R_i$ 由以下公式计算：

![img](/img/in-post/advertising/ri.png)

其中 $a$ 表示一个观察到的转化， $r_a$ 是转化的货币化价值， $P(x_i\|a)$ 是依据归因模型分配给通道 $x_i$ 的转化分数。对于mult-touch归因模型，转化分数计算方式为：

![img](/img/in-post/advertising/budget2.png)

其中 $V(x_j)$ 由之前提到的7.5式计算得到。

## Reference
[1] Display Advertising with Real-Time Bidding (RTB) and Behavioural Targeting by Jun Wang, Weinan Zhang and Shuai Yuan. ArXiv 2016.
