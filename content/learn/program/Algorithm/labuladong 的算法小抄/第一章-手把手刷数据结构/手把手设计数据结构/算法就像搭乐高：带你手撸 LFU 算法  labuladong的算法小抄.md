---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:25:46
tags: [learn/program/algorithm/labuladong/数据结构]
title: 算法就像搭乐高：带你手撸 LFU 算法  labuladong的算法小抄
---
# 算法就像搭乐高：带你手撸 LFU 算法 Labuladong的算法小抄
-   -   [一、算法描述](https://labuladong.gitee.io/algo/2/23/60/#%E4%B8%80%E7%AE%97%E6%B3%95%E6%8F%8F%E8%BF%B0)
    -   [二、思路分析](https://labuladong.gitee.io/algo/2/23/60/#%E4%BA%8C%E6%80%9D%E8%B7%AF%E5%88%86%E6%9E%90)

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

读完本文，你不仅学会了算法套路，还可以顺便解决如下题目：

| LeetCode | 力扣 | 难度 |
| --- | --- | --- |
| [460\. LFU Cache](https://leetcode.com/problems/lfu-cache/) | [460\. LFU 缓存](https://leetcode.cn/problems/lfu-cache/) | 🔴 |

**———–**

上篇文章 [带你手写LRU算法](https://labuladong.gitee.io/algo/2/23/59/) 写了 LRU 缓存淘汰算法的实现方法，本文来写另一个著名的缓存淘汰算法：LFU 算法。

LRU 算法的淘汰策略是 Least Recently Used，也就是每次淘汰那些最久没被使用的数据；而 LFU 算法的淘汰策略是 Least Frequently Used，也就是每次淘汰那些使用次数最少的数据。

LRU 算法的核心数据结构是使用哈希链表 `LinkedHashMap`，首先借助链表的有序性使得链表元素维持插入顺序，同时借助哈希映射的快速访问能力使得我们可以在 O(1) 时间访问链表的任意元素。

从实现难度上来说，LFU 算法的难度大于 LRU 算法，因为 LRU 算法相当于把数据按照时间排序，这个需求借助链表很自然就能实现，你一直从链表头部加入元素的话，越靠近头部的元素就是新的数据，越靠近尾部的元素就是旧的数据，我们进行缓存淘汰的时候只要简单地将尾部的元素淘汰掉就行了。

而 LFU 算法相当于是把数据按照访问频次进行排序，这个需求恐怕没有那么简单，而且还有一种情况，如果多个数据拥有相同的访问频次，我们就得删除最早插入的那个数据。也就是说 LFU 算法是淘汰访问频次最低的数据，如果访问频次最低的数据有多条，需要淘汰最旧的数据。

所以说 LFU 算法是要复杂很多的，而且经常出现在面试中，因为 LFU 缓存淘汰算法在工程实践中经常使用，也有可能是因为 LRU 算法太简单了。**不过话说回来，这种著名的算法的套路都是固定的，关键是由于逻辑较复杂，不容易写出漂亮且没有 bug 的代码**。

那么本文我就带你拆解 LFU 算法，自顶向下，逐步求精，就是解决复杂问题的不二法门。

## 一、算法描述

要求你写一个类，接受一个 `capacity` 参数，实现 `get` 和 `put` 方法：

```
class LFUCache {
    // 构造容量为 capacity 的缓存
    public LFUCache(int capacity) {}
    // 在缓存中查询 key
    public int get(int key) {}
    // 将 key 和 val 存入缓存
    public void put(int key, int val) {}
}
```

`get(key)` 方法会去缓存中查询键 `key`，如果 `key` 存在，则返回 `key` 对应的 `val`，否则返回 -1。

`put(key, value)` 方法插入或修改缓存。如果 `key` 已存在，则将它对应的值改为 `val`；如果 `key` 不存在，则插入键值对 `(key, val)`。

当缓存达到容量 `capacity` 时，则应该在插入新的键值对之前，删除使用频次（后文用 `freq` 表示）最低的键值对。如果 `freq` 最低的键值对有多个，则删除其中最旧的那个。

```
// 构造一个容量为 2 的 LFU 缓存
LFUCache cache = new LFUCache(2);

// 插入两对 (key, val)，对应的 freq 为 1
cache.put(1, 10);
cache.put(2, 20);

// 查询 key 为 1 对应的 val
// 返回 10，同时键 1 对应的 freq 变为 2
cache.get(1);

// 容量已满，淘汰 freq 最小的键 2
// 插入键值对 (3, 30)，对应的 freq 为 1
cache.put(3, 30);   

// 键 2 已经被淘汰删除，返回 -1
cache.get(2);       
```

## 二、思路分析

一定先从最简单的开始，根据 LFU 算法的逻辑，我们先列举出算法执行过程中的几个显而易见的事实：

1、调用 `get(key)` 方法时，要返回该 `key` 对应的 `val`。

2、只要用 `get` 或者 `put` 方法访问一次某个 `key`，该 `key` 的 `freq` 就要加一。

3、如果在容量满了的时候进行插入，则需要将 `freq` 最小的 `key` 删除，如果最小的 `freq` 对应多个 `key`，则删除其中最旧的那一个。

好的，我们希望能够在 O(1) 的时间内解决这些需求，可以使用基本数据结构来逐个击破：

1、使用一个 `HashMap` 存储 `key` 到 `val` 的映射，就可以快速计算 `get(key)`。

```
HashMap<Integer, Integer> keyToVal;
```

2、使用一个 `HashMap` 存储 `key` 到 `freq` 的映射，就可以快速操作 `key` 对应的 `freq`。

```
HashMap<Integer, Integer> keyToFreq;
```

3、这个需求应该是 LFU 算法的核心，所以我们分开说：

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

应合作方要求，本文不便在此发布，请扫码关注回复关键词「lfu」或 [点这里](https://appktavsiei5995.pc.xiaoe-tech.com/detail/i_627ceadae4b01c509aaf8afa/1) 查看：

[![](https://labuladong.gitee.io/algo/images/qrcode.jpg)](https://labuladong.gitee.io/algo/images/qrcode.jpg)

**共同维护高质量学习环境，评论礼仪[见这里](https://mp.weixin.qq.com/s/YdSoYZS0QjZpbphQlpHyyA)，违者直接拉黑不解释**