---
layout: post
title: "Display Advertising with
Real-Time Bidding (RTB) and
Behavioural Targeting读书笔记（六）"
subtitle: "欺诈检测"
author: "Wenqian"
header-mask: 0.4
mathjax: true
tags:
  - 机器学习
  - 广告算法
  - 实时竞价
---
## 广告欺诈类型
首先给广告欺诈下个定义（这里采用谷歌的说法）：

> 无法反应真实用户兴趣的无效流量（包括曝光、点击和转化）。

广告欺诈的类型大概可以分为三种（依据流量类型）：

1. **曝光欺诈**。欺诈者会生成假的竞价请求（以发布者的角度，即假的曝光机会），并在ADX内售卖它们以获取收益。
2. **点击欺诈**。欺诈者在加载广告后伪造点击行为。
3. **转化欺诈**。欺诈者在加载广告后伪造某种转化行为。

**通常不同类型的欺诈会同时发生**。

## 广告欺诈来源
### Pay-per-view（PPV）网络
PPV网络对应了上面说的**曝光欺诈**，即提供无效曝光。下图反应了PPV网络是如何制造流量的：

![img](/img/in-post/advertising/ppv.png)

PPV网络的运行模式是与合法的发布方（一般是多个）进行合作，支付给他们一定的费用来在网站上生成PPV网络的广告tag。**这些tag被用来生成看不见的隐性弹出窗口，这些窗口会加载目标网站（也就是受害者）**。通常这些窗口是在当前浏览器窗口的下方，并且大小为0x0像素，因此人们既很难发现，也很难关闭。由此我们也可以发现PPV网络的几个特点：

1. 购买的流量并不遵循通常的日夜周期，并且交互很少。
2. PPV的流量中存在很多不完整的加载行为，大概占比60%。
3. 提供非常多不同的IP。
4. 通常曝光的高度或宽度为0。

由此我们可以针对性设计一个检测和过滤系统。主要通过以下方式：

1. 曝光区域大小检查：有效的曝光是不可能只在一个0x0的区域内展示的。
2. 曝光提供者黑名单：查看该流量是否由PPV网络提供。
3. 发布者黑名单：和上面类似，只不过针对发布者。

### 机器人网络（Botnets）
机器人网络一般是通过植入木马的方式入侵并控制他人的电脑，并在此之后将这些受控电脑所产生的流量卖给发布方。之后就可以以类似于PPV网络的方式欺诈并获取广告主的佣金。

应对机器人网络的方式大概有三种：
1. 基于签名的检测，即用已知的机器人网络行为中提取软件或网络包签名。
2. 异常检测，机器人网络通常存在以下异常：高网络延迟、高网络流量、流量位于反常端口以及一些反常系统行为。
3. 基于DNS的检测，通过分析DNS流量来判别是否是机器人网络。

### 竞争者攻击
广告主会在在线广告系统中使用他们的预算来购买曝光和点击，而这也会给竞争者创造攻击的机会。竞争者可以故意加载和点击对手的广告来耗尽他们的预算。同时，由于广告系统中通常采用第二价格拍卖，他自己在之后的出价中也可以支付的更少。

### 其他来源
除了上面说的，其他来源还有：
1. 雇佣的spammer
2. 关键词装填，发布者通过向网页中装填无关的关键词（通常对于用户是不可见的）来获取更多广告收入。
3. 曝光装填，发布者叠加许多横幅（或者让它们不可见）来在每个页面上获取更多数量的曝光。
4. 胁迫，发布者要求用户去点广告来支持网站，或者把广告混淆在正常的内容里。
5. 强制的浏览器行为，攻击者强制用户的浏览器去加载多余的网页或者点击广告。

## 基于公共访问网络的广告欺诈检测
这种方法会**把用户和网站的关系看做是一个二部图**，而访问行为则是节点之间的边。它有个潜在的前提假设是：**某个网站的访问用户通常不会同时又访问相同的另一个网站**。一旦这种共同访问率（co-visitation rate）超过某个阈值（一般是50%），我们便可以认为这些网站构成了一个簇，该簇内的网站很可能都是恶意网站。

## 欺诈检测中的特征工程
这里简单介绍了一下在2012年移动广告欺诈检测（FDMA）比赛中各个参赛队伍所采用的一些特征工程手段。该比赛将欺诈检测看做是一个多分类任务，即预测一个发布者的状态是OK、Observation还是Fraud。人工构建的特征大概可以分为以下几种：

1. 不同时间段内的点击总数/均值以及标准差
2. 总体以及互不相同的引用URL数量；引用URL数量的均值和标准差
3. 总体以及互不相同的设备用户代理（Device User Agents）数量；设备用户代理数量的均值和标准差
4. 总体以及互不相同的IP数量；IP数量的均值和标准差
5. 国家、城市或更细粒度的用户位置信息

二阶特征可以进一步通过组合得到。比如浏览器为Chrome且day-part=Morning的点击总数。

## 可视性方法（Viewability methods）
这种方法关注于研究广告展示的比例和曝光时间对用户广告召回率的影响。研究人员发现只有当广告展示比例超过某个阈值（通常是75%），并连续曝光超过一定时间（通常是2秒）后，这次广告曝光才能被认为是有效的。一旦设置了这种条件，很多异常流量也就可以被过滤掉了。

## 其他方法
可以生成“钓鱼广告”投放到某个广告网络或发布者处（一般钓鱼广告包含没什么关联的展示信息，也就不会有什么人点击），看看CTR是不是过高。也可以在可信的网站排名机构处查看网站的流行度和排名，在对比发布者提供的流量，看看两者是否相符。如果发布者可提供的流量过高，那么就有可能有欺诈行为。

至此，本书的阅读基本完结。

## Reference
[1] Display Advertising with Real-Time Bidding (RTB) and Behavioural Targeting by Jun Wang, Weinan Zhang and Shuai Yuan. ArXiv 2016.