---
layout: post
title: "「算法总结系列」：栈"
subtitle: "Stack"
author: "Wenqian"
header-mask: 0.4
tags:
  - 算法
  - 栈
---

## 什么是栈

栈是一种后进先出（LIFO）的数据结构。它通常具备以下几种操作：（1）pop：提出栈顶元素。（2）push：将一个元素压进栈中。（3）peek：返回栈顶元素。（4）empty：判断栈是否为空。

## 什么时候会用到栈

一般来说有以下几种情况：
（1）明确的考察你是否真的理解栈这种数据结构，比如让你用栈来实现一些特殊功能，或者用栈来实现其他数据结构。例如：[Min Stack](https://leetcode.com/problems/min-stack/#/description)和[Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/#/description)。
（2）和括号有关的题目，像是判断括号是否有效，或者计算带括号的算式。比如[Valid Parentheses](https://leetcode.com/problems/valid-parentheses/#/description)和[Basic Calculator](https://leetcode.com/problems/basic-calculator/#/description)。
（3）树的遍历。通常需要你用循环的方式（而不是递归方式）按照某个顺序遍历树时要用到栈。例如[Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/#/description)和[Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/#/description)。
（4）和某一段单调序列有关。例如[Largest Rectangle in Histogram](https://leetcode.com/problems/largest-rectangle-in-histogram/#/description)，[Maximal Rectangle](https://leetcode.com/problems/maximal-rectangle/#/description)和[Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/#/description)。

## 如何实现栈

栈一般由数组或链表实现。但Java中有Stack类，所以不用自己实现。

## 栈操作的时间复杂度

上面提到的操作的时间复杂度均为O(1)。