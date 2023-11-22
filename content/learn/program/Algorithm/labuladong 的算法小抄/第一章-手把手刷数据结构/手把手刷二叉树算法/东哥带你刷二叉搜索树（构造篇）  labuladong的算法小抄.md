---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:21:18
tags: [learn/program/algorithm/labuladong/数据结构/二叉树]
title: 东哥带你刷二叉搜索树（构造篇）  labuladong的算法小抄
---
# 东哥带你刷二叉搜索树（构造篇） Labuladong的算法小抄
## 东哥带你刷二叉搜索树（构造篇）

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

读完本文，你不仅学会了算法套路，还可以顺便解决如下题目：

| LeetCode | 力扣 | 难度 |
| --- | --- | --- |
| [95\. Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii/) | [95\. 不同的二叉搜索树 II](https://leetcode.cn/problems/unique-binary-search-trees-ii/) | 🟠 |
| [96\. Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees/) | [96\. 不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/) | 🟠 |

**———–**

PS： [刷题插件](https://mp.weixin.qq.com/s/OE1zPVPj0V2o82N4HtLQbw) 集成了手把手刷二叉树功能，按照公式和套路讲解了 150 道二叉树题目，可手把手带你刷完二叉树分类的题目，迅速掌握递归思维。

之前写了两篇手把手刷 BST 算法题的文章， [第一篇](https://labuladong.gitee.io/algo/2/21/42/) 讲了中序遍历对 BST 的重要意义， [第二篇](https://labuladong.gitee.io/algo/2/21/43/) 写了 BST 的基本操作。

本文就来写手把手刷 BST 系列的第三篇，循序渐进地讲两道题，如何计算所有有效 BST。

第一道题是力扣第 96 题「 [不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/)」，给你输入一个正整数 `n`，请你计算，存储 `{1,2,3...,n}` 这些值共有多少种不同的 BST 结构。

函数签名如下：

比如说输入 `n = 3`，算法返回 5，因为共有如下 5 种不同的 BST 结构存储 `{1,2,3}`：

[![](https://labuladong.gitee.io/algo/images/BST3/2.jpg)](https://labuladong.gitee.io/algo/images/BST3/2.jpg)

这就是一个正宗的穷举问题，那么什么方式能够正确地穷举有效 BST 的数量呢？

我们前文说过，不要小看「穷举」，这是一件看起来简单但是比较有技术含量的事情，问题的关键就是不能数漏，也不能数多，你咋整？

之前 [手把手刷二叉树第一期](https://labuladong.gitee.io/algo/2/21/37/) 说过，二叉树算法的关键就在于明确根节点需要做什么，其实 BST 作为一种特殊的二叉树，核心思路也是一样的。

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

应合作方要求，本文不便在此发布，请扫码关注回复关键词「BST」或 [点这里](https://appktavsiei5995.pc.xiaoe-tech.com/detail/i_62987940e4b01a4852072f8c/1) 查看：

[![](https://labuladong.gitee.io/algo/images/qrcode.jpg)](https://labuladong.gitee.io/algo/images/qrcode.jpg)

**共同维护高质量学习环境，评论礼仪[见这里](https://mp.weixin.qq.com/s/YdSoYZS0QjZpbphQlpHyyA)，违者直接拉黑不解释**