---
layout: post
title: "「算法总结系列」：广度优先算法"
subtitle: "Breath First Search"
author: "Wenqian"
header-mask: 0.4
tags:
  - 算法
  - BFS
---

## 什么是广度优先算法

广度优先算法即BFS（Breath-First-Search），是一种图形搜索算法。它一般从一个根节点出发（有时是任意的一个节点），向外一层一层（同一层的节点到根节点的距离相同）的搜索，直到所有节点都被遍历为止。

## 什么时候会用到广度优先算法

之前也提到了，BFS是一种图形搜索算法，所以用到的BFS的题目一般也都与图相关。这个图可以是一个二维数组（每个点表示一个节点，相邻节点表示节点相连），也可以是一个树。
题目一般对图的广度比较在意，比如到起始点的距离或者树的高度。同时很多题其实也可以用DFS来解决，因为他们比较相近。
BFS本身其实并不难，我认为唯一难的地方就是根节点的选取。根节点的选取一般有以下几种：
（1）给定一个根节点或者任意选择根节点。这种就相对比较简单，比如[Clone Graph](https://leetcode.com/problems/clone-graph/#/description)。
（2）有多个根节点，且根节点的选取需要你自己判断。当然判断难度会不一样，有的比较简单，比如[Walls and Gates](https://leetcode.com/problems/walls-and-gates/#/description)；有的稍微难一些，比如[Minimum Height Trees](https://leetcode.com/problems/minimum-height-trees/#/description)和[Shortest Distance from All Buildings](https://leetcode.com/problems/shortest-distance-from-all-buildings/#/description)。
还有一些较难的综合题，比如结合BFS和DFS的[Word Ladder II](https://leetcode.com/problems/word-ladder-ii/#/description)。

## 如何实现广度优先算法

BFS的实现主要有以下几步：
1. 新建一个先入先出的队列，并将根节点放入队列中。
2. 从队列内取出第一个节点，判断他是否满足要求（包括未被遍历，以及一些每个题目都不一样的要求）。若满足则将其标记为已被遍历，进行某些操作（比如返回该节点，或对于节点的值进行一些运算，具体操作因题而异），并将他所有的子节点放入队列中；若未满足，则取出下一个节点。
3. 不断重复操作2，直到队列为空。
4. 返回最终结果（这一步也可能在操作2中完成）。

## 广度优先算法的时间复杂度

BFS的时间复杂度为O(|V| + |E|)，其中|V|是节点的数目，而|E|是图中边的数目。空间复杂度也是O(|V| + |E|)。

## 广度优先算法模板

```java
    // 新建一个队列
    LinkedList<Node> queue = new LinkedList<>();
    // 将根节点插入队列中
    queue.add(root);
    while (queue.size() > 0) {
        Node curr = queue.poll();
        int x = curr.x;
        int y = curr.y;
        // 如果满足条件
        if ( visited[x][y] != 1 && ... ) {
            visited[x][y] = 1;
            // 进行某些操作
            ...
        } else {
            continue;
        }
    }
    return ...
```