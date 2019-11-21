---
layout: post
title: "「算法总结系列」：堆"
subtitle: "Heap"
author: "Wenqian"
header-mask: 0.4
tags:
  - 算法
  - 堆
---

## 什么是堆

这里说的堆是一种数据结构，通常是由一个被看做树的数组构成。堆分为最大堆（Max-Heap）和最小堆（Min-Heap），他们的不同点在于最大堆的根节点为所有元素的最大值，同时它的左右子树也都是最大堆，而最小堆则恰恰相反，其根节点为所有元素的最小值，而左右子树也都是最小堆。下图是一个最大堆的例子。
![最大堆图例 图片来源：维基百科https://en.wikipedia.org/wiki/Heap_(data_structure)](https://upload.wikimedia.org/wikipedia/commons/3/38/Max-Heap.svg)

## 什么情况下会用到堆

我个人感觉以下几种情况通常会用到堆：

（1）题目直接让你构造一个堆
当然不能用已有的数据结构，比如Java的PriorityQueue。例题：[Heapify](http://www.lintcode.com/en/problem/heapify/)。

（2）题目要求求第k小（或大）的元素

相对简单的题目比如：[Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/#/description)和[Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/#/description)。这两道题都是很明显地将所有数都告诉你了，然后让你求第k小（或频率第k小）的数。那么很简单，就把所有的数放入一个最小堆，再不断弹出即可。
注意：这些题目都可以用堆来做，但不代表只能用堆来做。比如第k小的数也可以用quick select来求，而且平均时间复杂度要更小。

那么稍微难一点的题可能就不会明显的告诉你数组中有那些数，但是他会明示或暗示你一些顺序条件，让你能清楚地知道下一个最小数一定在某些数之间。那么你只要将所有的可能不断加入堆中，再将堆顶弹出即可。比如[Ugly Number II](https://leetcode.com/problems/ugly-number-ii/#/description)，[Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/#/description)，[Find K Pairs with Smallest Sums](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/#/description)和[Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/#/description)（这道题很好，因为用到了java中的很多模块）。

（3）和扫描线一起考
这部分会在扫描线的专题里详细表述。例题有：[Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/#/description)和[The Skyline Problem](https://leetcode.com/problems/the-skyline-problem/#/description)。

（4）中位数问题
用一个最大堆和一个最小堆分别表示比中位数小的所有数和比它大的所有数。当新加入一个数时，更新两个堆以及中位数即可。
例如：[Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/#/description)和[Sliding Window Median](http://www.lintcode.com/en/problem/sliding-window-median/)

## 如何实现堆

构造一个堆一般指的都是构造一个二叉堆（binary heap）（就像第一部分中的图那样），它的结构一般是数组。堆顶即为第一个元素，之后依次是它的左子节点和右子节点，以此类推直到最后一个叶子节点。

堆的操作一般有入堆，出堆和删除。
入堆即将某个元素插入堆中，这时我们需要先将元素放入数组末尾，然后将其不断与父节点比较，并进行交换操作，直到其到了正确的位置为止（这个过程也被叫做Sift Up）。
出堆即删除堆顶元素，那么先将堆顶元素与末尾元素对调，然后将末尾元素不断与其子节点进行比较和交换，直到其处于正确的位置上（这个过程也被叫做Sift Down）。
删除即删除某个元素，它包括查找和删除。因为堆只能保证堆顶是最大或最小，而不能保证其两个子树的大小关系，所以查找一般会耗费较多时间（O(n)）。查找到之后的删除部分与出堆类似，这里不再赘述。

## 时间复杂度

创建空堆：O(n)
插入一个元素：O(log n)
弹出堆顶：O(log n)
删除一个元素：O(n)
得到堆顶：O(1)

使用Hash heap可以将删除元素的时间复杂度从O(n)提升到O(log n)，其他不变。Hash heap详情见拓展部分。

## 堆模板

注意：模板代码以最小堆为例。

首先是自己构造一个堆(假设数组长度为n，堆内的元素个数为k，且k小于n)：

```java
    public class MinHeap {
        int[] minHeap;
        int size;
        int capability;

        public MinHeap(int capability) {
            size = 0;
            this.capability = capability;
            minHeap = new int[capability];
        }
        
        // Add an element to min heap
        public void add(int a) {
            minHeap[size] = a;
            if (size != 0) {
                int id = size;
                while (parent(id) > -1) {
                    int parentId = parent(id);
                    if (minHeap[parentId] > minHeap[id]) {
                        sweep(parentId, id);
                        id = parentId;
                    } else {
                        break;
                    }
                }
            }
            size++;
        }

        // Delete the peek element of min heap
        public int poll() {
            if (size == 0) return -1;
            sweep(0, size-1);
            int id = 0;
            // lson should be less than size-1(last index)
            while(2*id+1 < size-1) {
                int lson = 2*id + 1;
                int rson = lson + 1;
                int son;
                if (rson >= size-1 || minHeap[lson] < minHeap[rson]) {
                    son = lson;
                } else {
                    son = rson;
                }
                if (minHeap[son] > minHeap[id]) {
                    break;
                } else {
                    sweep(son, id);
                    id = son;
                }
            }           
            size--;
            return minHeap[size];
        }
        
        public int parent(int id) {
            return (id-1)/2;
        }

        public void sweep(int a, int b) {
            int temp = minHeap[a];
            minHeap[a] = minHeap[b];
            minHeap[b] = temp;
        }
    }
    
```

其次是运用Java自带的PriorityQueue(假设类是自建的Node)。

```java
    public class Node {
        int value;
        String s;
        public Node(int value, String s) {
            this.value = value;
            this.s = s;
        }
    }
    // Construct a priority queue of Node
    PriorityQueue<Node> minHeap = new PriorityQueue<Node>(capability, new Comparator<Node>() {
        // Sort by value
        public int compare(Node a, Node b) {
            return a.value - b.value;
        }
    });
```

## 拓展部分Hash-heap

其实就是用一个哈希表来记录heap的值和index，从而减少删除节点时查找部分的时间。