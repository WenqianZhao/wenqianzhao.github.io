---
layout: post
title: "Display Advertising with
Real-Time Bidding (RTB) and
Behavioural Targeting读书笔记（三）"
subtitle: "竞价策略"
author: "Wenqian"
header-mask: 0.4
mathjax: true
tags:
  - 机器学习
  - 广告算法
  - 实时竞价
---

竞价策略就是在给定一个广告展示机会时进行定价的逻辑。其方法分为两类，一类是以**博弈论**的视角去看，而另一类是以**统计学**的角度去看。这里将聚集在后一种方式，因为在实际情况下广告主和发布者很难说一定是有策略以及理性的，而这其实是博弈论视角的前提假设。如果以统计学角度来看，则一个竞价策略可以被抽象为一个以给定竞价请求为输入并以竞价价格为输出的映射方程：

![img](/img/in-post/advertising/bidding-strategy.png)

## 竞价的问题：RTB vs. 竞价排名（sponsored search）
之前的研究大多聚焦于**竞价排名场景下的关键词拍卖**任务，而**很少针对每次曝光的拍卖进行评估**，因而也就很少利用到曝光级别的特征，包括广告主和代理商的特征。另外搜索引擎既设置关键词竞价（策略），又主持拍卖，很容易让最终的优化目标由提升广告主的广告效果偏移到提升搜索引擎平台的收入。

与之相对的，RTB展示广告下的竞价优化就大不相同：

1. 首先报价是由曝光级别的特征决定，而不是事先定义好的关键词。
2. 在RTB中，通常采用CPM（千人成本）报价。

## RTB定量竞价（quantitative bidding）中的概念
这里给出了竞价策略的一个定量的分析框架：即**最终出价由效用（utility）和成本（cost）综合决定**。效用可以理解为**“得”**，包括像用户点击概率、转化概率、从这次曝光中获得的期望收入等；成本可以理解为**“失”**，即获得这次广告曝光机会需要付出什么，一般也就是预估的赢得这次曝光所需要的出价。图示如下：

![img](/img/in-post/advertising/quantitative-bidding.png)

**依赖CTR/CVR预估（对应utility）以及bid landscape预测（对应cost）技术，我们就可以搜寻得到最优的竞价策略**。假设定义一个竞价方程 $b(r)$ ，给定某次曝光的预估CTR $r$ ，返回其对应的报价。那么从广告主的角度出发，给定市场价格分布 $p\_{z}(z)$ 以及出价价格 $b$ ，获得此次拍卖胜利的概率为：

![img](/img/in-post/advertising/formula16.png)

如果拍卖获胜，则可以得到对应的效用和成本。效用方程 $u(r)$ 的形式由其KPI决定，例如当KPI为点击次数时，则：

![img](/img/in-post/advertising/formula17.png)

如果KPI是campaign的利润，且广告主对于每次点击的真实收益是 $v$ ,则：

![img](/img/in-post/advertising/formula18.png)

在成本部分，我们同样定义一个方程 $c(b)$ 表示竞价 $b$ 的成本。对于第一价格拍卖，其成本为：

![img](/img/in-post/advertising/formula19.png)

而对于第二价格拍卖则是：

![img](/img/in-post/advertising/formula20.png)

此外，campaign的设置一般包括特定的定向规则，寿命（lifetime）以及预算。其中campaign的定向规则和寿命决定了拍卖量（auction volume） $T$ ，而campaign的预算 $B$ 则规定了在整个寿命内其开销的上界。下面来具体看看几种竞价策略。

### Truth-telling bidding
在**不考虑竞价预算以及拍卖量的情况下，我们可以采用truth-telling竞价策略**。对于任意的预估CTR $r$ ，为了使利益最大化，我们可以出价：

![img](/img/in-post/advertising/formula21.png)

### 线性出价
如果考虑预算以及拍卖量，就不能直接使用truth-telling竞价策略了。线性出价策略其实很类似，就是在前面加了一个可调的系数（根据具体限制来调整）：

![img](/img/in-post/advertising/formula22.png)

### 预算限制下的点击率和转化率最大化
假设广告主想找到最优的竞价策略 $b()$ ，可以在整个campaign寿命内最大化campaign-level的KPI $r$ ，并且受限于总的出价请求量 $T$ 以及预算 $B$ ，即：

![img](/img/in-post/advertising/formula23.png)

为了解决这个问题可以利用拉格朗日乘子法得到：

![img](/img/in-post/advertising/formula24.png)

之后为了解出 $b(r)$ ，可以令loss对其的偏导数为0，从而得到：

![img](/img/in-post/advertising/formula25.png)

可见其实由 $w(b)$ 、 $u(r)$ 和 $c(b)$ 的具体形式决定。为了得到 $\lambda$ 其实也是类似：

![img](/img/in-post/advertising/formula26.png)

**在第一价格拍卖中**，上面的式5.15可以表示为：

![img](/img/in-post/advertising/formula27.png)

如果有了市场价格分布 $p\_{z}(z)$ 的具体形式我们就可以求得 $b()$ 。例如（感觉 $l$ 是个常数）：

![img](/img/in-post/advertising/formula28.png)

假设KPI为点击次数（也就是上面的式5.2），我们可以进一步得到：

![img](/img/in-post/advertising/formula29.png)

> 注意：上式5.21的右侧其实打错了，应该是 $\frac{l}{(l + b(r))^{2}}$。

如果市场价值分布是一个 $[0,l]$ 之间的均匀分布，那么可以推出最优出价的方程为：

![img](/img/in-post/advertising/formula30.png)

不过我觉得这个结论意义不大，因为市场价值的分布就不可能是一个区间内的均匀分布（假设性太强，或者说简化了太多）。

上面说的是第一价格拍卖，那么如果是第二价格拍卖，则上面的式5.15就变为：

![img](/img/in-post/advertising/formula31.png)

也就是说**最优出价方程是和CTR $r$ 呈线性关系的**。出奇的简单，并且正好和之前的线性出价对上了。而 $\lambda$ 可以通过下面的方式求得：

![img](/img/in-post/advertising/formula32.png)

这玩意虽然不太容易求出具体形式，但是好在 $c$ 和 $w$ 都是单调的（对于 $\lambda$ 而言），因此可以比较容易地得到一个数值解。

假设p同样服从均匀分布，CTR也是服从0-1之间的均匀分布，并且 $u(r) = r$ ， 那么可以直接求出：

![img](/img/in-post/advertising/formula33.png)

![img](/img/in-post/advertising/formula34.png)

通过上面的讨论我们可以看到竞价策略主要由三个因素影响：**广告主的预期效用，预算限制以及市场信息**。在第一价格拍卖中，效用方程和市场价格联合决定了竞价策略，而在第二价格拍卖中，竞价策略则是更多地由效用本身决定。

## 多活动（Multi-campaign）统计套利挖掘
在多活动场景中，广告主为了降低自身风险可能会要求仅对每次点击或转换来付费。而对于DSP来说，它的目标可能是在一个以曝光为结算准则的实时竞价系统里尽可能地获取更多的点击和转换。在这中间可能会存在一个gap，即**用户的出价会大于实际所需的价格（即赢得一次曝光的出价价格）**，这样也就出现了所谓的**套利空间**，对应的DSP被称作元进价者（meta-bidder）。多活动下的竞价最优框架可以写作：

![img](/img/in-post/advertising/formula35.png)

这里假设活动采样（campaign sampling）是一个随机过程，即采样到活动 $i$ 的概率是 $s_i$ ，并且和为1。对于一个固定的训练集我们可以算出元竞价者的收益和成本：

![img](/img/in-post/advertising/formula36.png)

之后把 $R$ 和 $C$ 看做是一个随机过程，那么优化的目标和限制也就可以转化成和 $R$ 与 $C$ 的期望和方差有关：

![img](/img/in-post/advertising/formula37.png)

并最终通过一种EM形式的算法来解决。

## 预算调整（Budget pacing）
预算调整的方法大概分为节流（throttling）和竞价调整（bid modification）两类。节流的方式其实就是把活动每天的时间切分成 $T$ 份，然后保证每个时间区间对应的开销 $b_t$ 的总和为 $B$ ，同时又最大化业务指标。竞价调整方法则是利用基于反馈控制的算法去动态调整出价。反馈控制器通常嵌入在DSP的竞价代理处。

相比于节流策略，竞价调整方法会更灵活地应对各种各样的预算调整目标。但是这种出价控制方法很大程度上依赖于市场竞争的预测精度，也就是之前讨论过的bid landscape预测。相比之下，节流方法更直接一些，在动态市场中通常表现得更好。

## Reference
[1] Display Advertising with Real-Time Bidding (RTB) and Behavioural Targeting by Jun Wang, Weinan Zhang and Shuai Yuan. ArXiv 2016.
