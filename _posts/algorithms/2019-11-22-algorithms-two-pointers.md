---
layout: post
title: "「算法总结系列」：双指针"
subtitle: "Two Pointers"
author: "Wenqian"
header-mask: 0.4
tags:
  - 算法
  - 双指针
---

## 什么是两个指针

两个指针指的是在解题过程中我们会先声明两个指针left和right，分别指向目标（通常是数组）的两点（通常是同一点或者数组的两端），指针如何移动，哪个指针应该移动通常是由两个指针指向的点所决定的。不断移动两个指针直到某个条件不满足为止（比如`left > right`或者`right >= length`等等）。

## 什么情况下会用到两个指针

两个指针一般分为以下几种情况：
1. 对撞型指针。即两个指针分别从两头向中间聚集，当两个指针相遇时结束。
例如：[Trapping Rain Water](http://www.lintcode.com/en/problem/trapping-rain-water/)，[Container With Most Water](http://www.lintcode.com/en/problem/container-with-most-water/)，经典题 [3Sum](https://leetcode.com/problems/3sum/#/description)。
2. 同向型指针。即两个指针的行动方向相同，当某个指针超出边界或相遇时结束。
这种类型也可以细分为以下两种小类：
（1） 窗口型。通常是string的substring。
例如：[Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/#/description)，[Minimum Size Subarray Sum](http://www.lintcode.com/en/problem/minimum-size-subarray-sum/)，[Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/#/description)，[Longest Substring with At Most K Distinct Characters](http://www.lintcode.com/en/problem/longest-substring-with-at-most-k-distinct-characters/)。
（2） 追赶型。两个指针一前一后互相追赶，指针的移动速度不一定相同。
例如：[Move Zeroes](https://leetcode.com/problems/move-zeroes/#/description)，[Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/#/description)（这道题很有意思）。
3. 并行型指针。上面说的都是同一个数组存在两个指针，这里则不一样。并行型指针通常是指有两个数组，每个数组分别有一个指针不断移动，直到某个条件满足时结束。
例如：[Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/#/description)。