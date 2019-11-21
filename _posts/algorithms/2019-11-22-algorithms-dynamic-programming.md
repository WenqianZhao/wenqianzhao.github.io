---
layout: post
title: "「算法总结系列」：动态规划"
subtitle: "Dynamic Programming"
author: "Wenqian"
header-mask: 0.4
tags:
  - 算法
  - 动态规划
---

## 什么是动态规划

动态规划往往应用于最优化问题，即做出一组选择从而得到一个最优解（可以同时存在多个最优解）。一组选择作为一个整体可以被分化成一个个子问题，而这些子问题中往往存在重复，我们要做的就是保存不同的子问题结果从而在遇到重复问题时可以节省时间。最终由一个个子问题的结果自下而上得出最后的最优解。

## 动态规划的类型
（1） 序列型动态规划。构造一个一维或二维数组来顺序的存放中间结果。这类题目通常可以用滚动数组来优化空间复杂度。
例如：[House Robber](https://leetcode.com/problems/house-robber/#/description)和[Maximal Square](https://leetcode.com/problems/maximal-square/#/description)。
复杂的可能会构造两个一维数组同时更新：[Wiggle Subsequence](https://leetcode.com/problems/wiggle-subsequence/#/description)。
（2） 记忆化搜索。在搜索过程中储存一些中间结果，以避免重复计算。
例如：[Longest Increasing Continuous subsequence II](http://www.lintcode.com/en/problem/longest-increasing-continuous-subsequence-ii/)和[Word Break II](https://leetcode.com/problems/word-break-ii/#/description)。
（3） 博弈类动态规划。
（4） 区间类动态规划。这类题一般都是求一个区间的max,min或是count值。从大到小进行思考，转移方程通过区间更新。
例如：[Stone Game](http://www.lintcode.com/en/problem/stone-game/)和[Burst Balloons](https://leetcode.com/problems/burst-balloons/#/description)。
（5） 背包类动态规划。
（6） 网格类动态规划。一般是在两个字符串之间进行，假如一个字符串长度为a，另一个为b，那么将生成一个a*b大小的网格，dp[i][j]存放前一个字符串的前i个字符和后一个字符串的前j个字符之间的某种关系。
例如：[Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/#/description)，[Edit Distance](https://leetcode.com/problems/edit-distance/#/description)和[Distinct Subsequences](https://leetcode.com/problems/distinct-subsequences/#/description)。

## 如何实现动态规划

这里引用算法导论的描述：
> 动态规划算法的设计可以分为以下4个步骤：
> 1. 描述最优解的结构。
> 2. 递归定义最优解的值。
> 3. 按自底向上的方式计算最优解的值。
> 4. 由计算出的结果构造一个最优解。
> 第1~3步构成问题的动态规划解的基础。第4步在只要求计算最优解的值时可以略去。如果的确做了第4步，则有时要在第3步的计算中记录一些附加信息，使得构造一个最优解变得容易。

