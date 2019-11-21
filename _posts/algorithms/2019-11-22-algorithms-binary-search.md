---
layout: post
title: "「算法总结系列」：二分搜索法"
subtitle: "Binary Search"
author: "Wenqian"
header-mask: 0.4
tags:
  - 算法
  - 二分搜索法
---

## 什么是二分法搜索

通过二分方法对一个排序过的数组进行搜索，判断某个数是否在数组内。所谓的二分方法及不断将一段数中的中间值与目标值进行比较，如果目标值较大，就将搜索区域缩小为中间值的右侧，如果目标值较小，则将搜索区域缩小为中间值的右侧。如此反复进行，直到找到目标值或者搜索区域缩小为0。

## 什么时候会用到二分法搜索

使用二分法搜索有一个前提，那就是数组一定要是排序好的。因此一般用到二分法的题目有两个特点：一是数组已排序，二是需要寻找某个数。
（1）第一种是很明显的告诉你数组和我要找的数。例如：[Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/#/description)和[Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/#/description)。
（2）如果对排序后的数组进行了一些简单的处理，也可以用二分法搜索。例如：[Find Minimum in Rotated Sorted Array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/#/description)和[Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/#/description)。
（3）排序过的可能不是数组，而是树（BST）。例如：[Kth Smallest Element in a BST](https://leetcode.com/problems/kth-smallest-element-in-a-bst/#/description)。
（4）所谓的排序过的数组可能是一个整数的区间范围。例如：[Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/#/description)。
（5）合并两个排序过的数组，并寻找某个数。例如：[Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/#/description)。
（6）利用二分法搜索的性质，自己构建一个递增的数组，比较难想到。例如：[Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/#/description)。

## 如何实现二分法搜索

二分法搜索的过程中需要维护搜索区域的左边界和右边界（我一般将它标为left和right）。这两个值可以是数组的index，也可以是搜索区域的边界值。当left < right时不断重复以下操作：
1. 通过left和right计算出他们的中间值mid。
2. 将mid（或它对应的数）与目标值比较，并根据比较结果对left或right的值进行更新。当然，如果mid直接满足要求，就返回。

## 二分法搜索的时间复杂度

一般是O(logN)，N是数组长度。