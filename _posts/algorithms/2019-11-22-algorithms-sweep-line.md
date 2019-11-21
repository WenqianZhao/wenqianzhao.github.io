---
layout: post
title: "「算法总结系列」：扫描线"
subtitle: "Sweep Line"
author: "Wenqian"
header-mask: 0.4
tags:
  - 算法
  - 扫描线
---

## 什么是扫描线算法

这里所说的扫描线算法其实是一种简化，特指用一根与x轴相垂直的直线扫过整个平面时，根据其与平面上其他线的交点计算最终我们需要的结果。

## 什么时候会用到扫描线算法

扫描线算法一般用在区间问题上。比如最经典的[Number of Airplanes in the Sky](http://www.lintcode.com/en/problem/number-of-airplanes-in-the-sky/)，题目给你几个区间分别表示不同航班的起飞和降落时间，问你天上最多同时有几架飞机。那么我们以时间为x轴，飞机数量为y轴，那么扫描线扫过整个区间后即可得到最多的飞机数量。
扫描线算法也经常和heap相结合，比如[Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/#/description)和[The Skyline Problem](https://leetcode.com/problems/the-skyline-problem/#/description)。

## 如何运用扫描线算法

扫描线算法通常要先将所有区间的起始点和结束点分开，然后对所有起始结束点进行排序。排序完成后，从第一个点开始进行扫描，期间要先判断是起始点还是结束点，并进行不同操作，知道所有点扫描结束为止。

## 时间复杂度

其实并没有一个标准的时间复杂度，不同问题时间复杂度可能也不同，因为一般要对所有区间端点进行排序，所以时间复杂度通常为O(nlogn)。