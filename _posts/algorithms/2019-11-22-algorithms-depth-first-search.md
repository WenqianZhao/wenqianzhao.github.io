---
layout: post
title: "「算法总结系列」：深度优先算法"
subtitle: "Depth First Search"
author: "Wenqian"
header-mask: 0.4
tags:
  - 算法
  - DFS
---

## 什么是深度优先算法

深度优先算法同BFS类似，也是一种用来遍历或搜索树或图的方法。与BFS不同的是，它更注重于“深度”，即尽可能深的搜索树的分支，只有当某个分支被完全搜索完后，他才会返回上一个节点并搜索下一个分支，直到所有分支都被搜索完后才结束。

## 什么时候会用到深度优先算法

（1）对树的深度很感兴趣时，可以用dfs。例如[Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/#/description)。
（2）对某条路径很感兴趣时，可以用dfs。例如[Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/#/description)，[Path Sum](https://leetcode.com/problems/path-sum/#/description)。
（3）对某条路径内的两点很感兴趣时，可以用dfs。例如[Recover Binary Search Tree](https://leetcode.com/problems/recover-binary-search-tree/#/description)。

## 如何实现深度优先算法

1. DFS一般都是采用递归的方式，不断调用自己。假设我们有一个子程序叫dfs()。那么第一步就是对根节点调用dfs，即dfs(root)。
2. 在dfs内一般先写好返回的条件以及返回的内容。之后如果不满足返回条件，则继续对当前节点的所有子节点调用自身。假设是二叉树，那么将会有两次调用（假设左右子节点都存在）。两次调用之间可能会加一些操作，调用结束后可能直接返回，也可能进行一些操作，因题而异。

## 广度优先算法的时间复杂度

时间复杂度O(b^m)，空间复杂度O(bm)。b是分支系数，m是图的最大深度。

