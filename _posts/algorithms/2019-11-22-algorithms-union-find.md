---
layout: post
title: "「算法总结系列」：并查集"
subtitle: "Union Find"
author: "Wenqian"
header-mask: 0.4
tags:
  - 算法
  - 并查集
---

## 什么是并查集

并查集（Union Find）其实就是一种将元素归类的方法。它由`归并（Union）`和`查找（Find）`两部分组成。其中`查找`部分就是查找某个元素所在的集合，而`归并`部分则是将两个元素所在的集合进行合并（如果两个元素已经在同一集合中，则不做任何操作）。

## 什么情况下会用到并查集

并查集一般用在将元素进行分类的情形下，而且分类的标准是元素是否“连通”。比如：
1. 连通图。题目可能给你一个连通图，或者干脆让你设计连通或断开的函数来生成一个连通图。之后可能会让你判断两点是否连通，或者连通区域的大小之类的。当然也可能给你一个图，让你判断是否连通（不一定会明说，但是中心思想是这个）。
例题：[Connecting Graph](https://www.lintcode.com/en/problem/connecting-graph/)，[Connecting Graph II](https://www.lintcode.com/en/problem/connecting-graph-ii/)，[Connecting Graph III](https://www.lintcode.com/en/problem/connecting-graph-iii/)，[Graph Valid Tree](https://www.lintcode.com/en/problem/graph-valid-tree/)
2. 连通图的衍生，比如与实际生活结合而生的变种。其中最典型的就是海岛问题。给你一个二维数组，用1和0（或者类似的变种）分别代表陆地和海洋，问你最大的海岛的面积，或者海岛数量等等。
例题：[Number of Islands](https://www.lintcode.com/en/problem/number-of-islands/)， [Number of Islands II](https://www.lintcode.com/en/problem/number-of-islands-ii/)，[Surrounded Regions](https://leetcode.com/problems/surrounded-regions/#/description)
3. 连续数字。比如判断一个数组中最长的连续数字是多长。
例题：[Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/#/description)

## 如何实现并查集

实现并查集最简单的方法是使用数组。假设数组名为father，那么father[n]代表的就是与n相连的节点，或者n所在集合的根节点。所以查找其实就是找出n所在集合的根节点，而归并则是首先判断两个节点所在集合的根节点是否一致，如果不一致则将两个集合的根节点统一（也就是进行了合并）。
那么如何判断某个节点是否是根节点呢？一般有两种方法。一是将根节点指向0，即`father[root] == 0`。这种方法的好处是初始化比较简单，但有时候因为存在0节点，所以会有歧义（消除这种歧义有时会比较麻烦，而且容易忽略）。第二种方法是将根节点指向自身。这种方法的好处是不用考虑什么歧义的问题，坏处是初始化稍微麻烦点。不过我个人还是倾向于使用第二种方法。
具体操作见模板部分。

## 时间复杂度

查找的时间复杂度为：O(1)
归并的时间复杂度为：O(1)

## 并查集模板

这里的模板中所有根节点的判断都是采用上文中的第二种方法。

查找部分：
```java
public int find(int a, int[] father) {
    if (father[a] == a) return a;
    else return father[a] = find(father[a], father);
}
```

归并部分：
```java
public void union(int a, int b, int[] father) {
    int root_a = find(a, father);
    int root_b = find(b, father);
    if (root_a != root_b) {
        father[root_a] = root_b;
    }
}
```