---
layout: post
title: "「算法总结系列」：Trie树"
subtitle: "Trie Tree"
author: "Wenqian"
header-mask: 0.4
tags:
  - 算法
  - Trie树
---

## 什么是Trie树

Trie树又叫前缀树或字典树，是一种有序树，它的键值通常是字符串。其实前面所说的“前缀树”和“字典树”已经非常形象地将Trie树的特点阐述了出来。
为什么叫它“前缀树”？这是因为Trie树中某个节点的所有子孙都拥有相同的前缀，也就是说通过Trie数我们可以很方便地知道这些词中存在哪些前缀。
为什么叫它“字典树”？这是因为通常我们会将一组词建立一个Trie树，而Trie树查找词的方式和我们平时在字典中查找词的方式是一样的（其实也就是通过前缀查找），也就是说Trie树看上去就像是一个字典。

## 什么情况下会用到Trie树

除了题目中明确让你建立一个Trie树以外（[Leetcode：Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/#/description)），Trie树一般用在对前缀比较敏感的地方。
什么叫对前缀敏感呢？一是题目中需要你根据前缀进行一些查找或处理，这里的前缀不一定是字母，也可能是数字或其他东西。二是题目可能需要你对一组词进行遍历，但在遍历过程中词的前缀信息可能会减少你的算法复杂度。
典型的题目有：[Maximum XOR of Two Numbers in an Array](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/#/description)， [Palindrome Pairs](https://leetcode.com/problems/palindrome-pairs/#/description)， [Word Search II](https://www.lintcode.com/en/problem/word-search-ii/)和[Word Squares](https://www.lintcode.com/en/problem/word-squares/)。

## 如何实现Trie树

对于简单的题目我们并不需要建立一个名为Trie的class，我们只需建立一个class用来创建单个节点，比如命名为TrieNode。TrieNode中一般会有一个boolean值isWord用来表示该节点是否为一个单词的结尾，一个类型同为TrieNode的数组next用来存放下一个可能的节点。如果我们已知字符只能是a-z（通常是这样），那么数组next的长度可以设为26.
对于不同的题目我们通常要对TrieNode的结构进行微调。比如加入所有前缀，或者以它结尾的词的index，再或者它之前的前缀是否是回文串等等。
有时可能还需要一个父类Trie，存放一些其他的信息或者一些对TrieNode进行操作的函数。
很多情况下，具体问题具体分析，这也是为什么Trie树的题目一般比较难的原因。

## 时间复杂度

建立Trie树的时间复杂度为：O(n*k) （n是单词个数，k是单词平均长度）

## Trie树模板

其实之前也说了，Trie树在不同题目中结构一般都是不一样的，所以并没有什么模板，不过这里还是给出一个Trie树的简单形式，以供参考。

```java
    public class TrieNode {
        boolean isWord;
        TrieNode[] next = new TrieNode[26];
        public TrieNode(boolean isWord) {
            this.isWord = isWord;
        }
    }
```