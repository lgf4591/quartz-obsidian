---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:25:17
tags: [learn/program/algorithm/labuladong/数据结构]
title: 一道求中位数的算法题把我整不会了  labuladong的算法小抄
---
# 一道求中位数的算法题把我整不会了 Labuladong的算法小抄
-   -   [尝试分析](https://labuladong.gitee.io/algo/2/23/62/#%E5%B0%9D%E8%AF%95%E5%88%86%E6%9E%90)

## 一道求中位数的算法题把我整不会了

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

读完本文，你不仅学会了算法套路，还可以顺便解决如下题目：

| LeetCode | 力扣 | 难度 |
| --- | --- | --- |
| [295\. Find Median from Data Stream](https://leetcode.com/problems/find-median-from-data-stream/) | [295\. 数据流的中位数](https://leetcode.cn/problems/find-median-from-data-stream/) | 🔴 |
| \- | [剑指 Offer 41. 数据流中的中位数](https://leetcode.cn/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/) | 🔴 |

**———–**

如果输入一个数组，让你求中位数，这个好办，排个序，如果数组长度是奇数，最中间的一个元素就是中位数，如果数组长度是偶数，最中间两个元素的平均数作为中位数。

如果数据规模非常巨大，排序不太现实，那么也可以使用概率算法，随机抽取一部分数据，排序，求中位数，作为所有数据的中位数。

本文说的中位数算法比较困难，也比较精妙，是力扣第 295 题「 [数据流的中位数](https://leetcode.cn/problems/find-median-from-data-stream/)」：

[![](https://labuladong.gitee.io/algo/images/%e4%b8%ad%e4%bd%8d%e6%95%b0/title.png)](https://labuladong.gitee.io/algo/images/%e4%b8%ad%e4%bd%8d%e6%95%b0/title.png)

就是让你设计这样一个类：

```
class MedianFinder {

    // 添加一个数字
    public void addNum(int num) {}

    // 计算当前添加的所有数字的中位数
    public double findMedian() {}
}
```

**其实，所有关于「流」的算法都比较难**，比如我在后文 [谈谈游戏中的随机算法](https://labuladong.gitee.io/algo/4/32/113/) 写过如何从数据流中等概率随机抽取一个元素，如果说你没有接触过这个问题的话，还是很难想到解法的。

这道题要求在数据流中计算平均数，我们先想一想常规思路。

### 尝试分析

一个直接的解法可以用一个数组记录所有 `addNum` 添加进来的数字，通过插入排序的逻辑保证数组中的元素有序，当调用 `findMedian` 方法时，可以通过数组索引直接计算中位数。

但是用数组作为底层容器的问题也很明显，`addNum` 搜索插入位置的时候可以用二分搜索算法，但是插入操作需要搬移数据，所以最坏时间复杂度为 O(N)。

那换链表？链表插入元素很快，但是查找插入位置的时候只能线性遍历，最坏时间复杂度还是 O(N)，而且 `findMedian` 方法也需要遍历寻找中间索引，最坏时间复杂度也是 O(N)。

那么就用平衡二叉树呗，增删查改复杂度都是 O(logN)，这样总行了吧？

比如用 Java 提供的 `TreeSet` 容器，底层是红黑树，`addNum` 直接插入，`findMedian` 可以通过当前元素的个数推出计算中位数的元素的排名。

很遗憾，依然不行，这里有两个问题：

第一，`TreeSet` 是一种 `Set`，其中不存在重复元素的元素，但是我们的数据流可能输入重复数据的，而且计算中位数也是需要算上重复元素的。

第二，`TreeSet` 并没有实现一个通过排名快速计算元素的 API。假设我想找到 `TreeSet` 中第 5 大的元素，并没有一个现成可用的方法实现这个需求。

> PS：如果让你实现一个在二叉搜索树中通过排名计算对应元素的方法 `select(int index)`，你会怎么设计？你可以思考一下，我会把答案写在留言区置顶。

除了平衡二叉树，还有没有什么常用的数据结构是动态有序的？优先级队列（二叉堆）行不行？

好像也不太行，因为优先级队列是一种受限的数据结构，只能从堆顶添加/删除元素，我们的 `addNum` 方法可以从堆顶插入元素，但是 `findMedian` 函数需要从数据中间取，这个功能优先级队列是没办法提供的。

可以看到，求个中位数还是挺难的，我们使尽浑身解数都没有一个高效地思路，下面直接来看解法吧，比较巧妙。

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

应合作方要求，本文不便在此发布，请扫码关注回复关键词「中位数」或 [点这里](https://appktavsiei5995.pc.xiaoe-tech.com/detail/i_62987949e4b01c509ab8b6af/1) 查看：

[![](https://labuladong.gitee.io/algo/images/qrcode.jpg)](https://labuladong.gitee.io/algo/images/qrcode.jpg)

**共同维护高质量学习环境，评论礼仪[见这里](https://mp.weixin.qq.com/s/YdSoYZS0QjZpbphQlpHyyA)，违者直接拉黑不解释**