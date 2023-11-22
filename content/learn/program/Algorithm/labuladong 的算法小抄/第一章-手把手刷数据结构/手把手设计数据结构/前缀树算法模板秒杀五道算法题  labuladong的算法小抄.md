---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:25:27
tags: [learn/program/algorithm/labuladong/数据结构]
title: 前缀树算法模板秒杀五道算法题  labuladong的算法小抄
---
# 前缀树算法模板秒杀五道算法题 Labuladong的算法小抄
×

由于本站访问压力较大  
微信扫码关注公众号 **labuladong**  
回复关键词「**解锁**」  
按照操作即可解锁本站全部文章  

[![公众号](https://labuladong.gitee.io/algo/images/qrcode.jpg)](https://labuladong.gitee.io/algo/images/qrcode.jpg)

## 前缀树算法模板秒杀五道算法题

[![GitHub](https://img.shields.io/github/stars/labuladong/fucking-algorithm?label=Stars&style=flat&logo=GitHub)](https://github.com/labuladong/fucking-algorithm)[![](https://img.shields.io/badge/B%E7%AB%99-@labuladong-000000.svg?style=flat&logo=Bilibili)](https://space.bilibili.com/14089380)[![](https://img.shields.io/static/v1?label=%E9%85%8D%E5%A5%97PDF%E5%92%8C%E6%8F%92%E4%BB%B6&message=%E4%B8%8B%E8%BD%BD&color=red&style=flat)](https://mp.weixin.qq.com/s/X-fE9sR4BLi6T9pn7xP4pg)[![](https://img.shields.io/static/v1?label=%E6%89%93%E5%8D%A1%E6%8C%91%E6%88%98&message=%E6%8A%A5%E5%90%8D&color=green&style=flat)](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q)[![](https://img.shields.io/static/v1?label=%E7%B2%BE%E5%93%81%E8%AF%BE%E7%A8%8B&message=%E6%9F%A5%E7%9C%8B&color=pink&style=flat)](https://appktavsiei5995.pc.xiaoe-tech.com/index)

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

读完本文，你不仅学会了算法套路，还可以顺便解决如下题目：

| LeetCode | 力扣 | 难度 |
| --- | --- | --- |
| [1804\. Implement Trie II (Prefix Tree)](https://leetcode.com/problems/implement-trie-ii-prefix-tree/)🔒 | [1804\. 实现 Trie （前缀树） II](https://leetcode.cn/problems/implement-trie-ii-prefix-tree/)🔒 | 🟠 |
| [208\. Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/) | [208\. 实现 Trie (前缀树)](https://leetcode.cn/problems/implement-trie-prefix-tree/) | 🟠 |
| [211\. Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/) | [211\. 添加与搜索单词 - 数据结构设计](https://leetcode.cn/problems/design-add-and-search-words-data-structure/) | 🟠 |
| [648\. Replace Words](https://leetcode.com/problems/replace-words/) | [648\. 单词替换](https://leetcode.cn/problems/replace-words/) | 🟠 |
| [677\. Map Sum Pairs](https://leetcode.com/problems/map-sum-pairs/) | [677\. 键值映射](https://leetcode.cn/problems/map-sum-pairs/) | 🟠 |
| \- | [剑指 Offer II 062. 实现前缀树](https://leetcode.cn/problems/QC3q1f/) | 🟠 |
| \- | [剑指 Offer II 063. 替换单词](https://leetcode.cn/problems/UhWRSj/) | 🟠 |
| \- | [剑指 Offer II 066. 单词之和](https://leetcode.cn/problems/z1R5dt/) | 🟠 |

**———–**

> 本文有视频版： [动手实现字典树](https://appktavsiei5995.pc.xiaoe-tech.com/detail/p_6265562be4b01c509aa7b629/6)

Trie 树又叫字典树、前缀树、单词查找树，是一种二叉树衍生出来的高级数据结构，主要应用场景是处理字符串前缀相关的操作。

我是在《算法 4》第一次学到这种数据结构，不过书中的讲解不是特别通俗易懂，所以本文按照我的逻辑帮大家重新梳理一遍 Trie 树的原理，并基于《算法 4》的代码实现一套更通用易懂的代码模板，用于处理力扣上一系列字符串前缀问题。

阅读本文之前希望你读过我旧文讲过的 [回溯算法代码模板](https://labuladong.gitee.io/algo/4/31/104/) 和 [手把手刷二叉树（总结篇）](https://labuladong.gitee.io/algo/2/21/36/)。

本文主要分三部分：

**1、讲解 Trie 树的工作原理**。

**2、给出一套 `TrieMap` 和 `TrieSet` 的代码模板，实现几个常用 API**。

**3、实践环节，直接套代码模板秒杀 5 道算法题**。本来可以秒杀七八道题，篇幅考虑，剩下的我集成到 [刷题插件](https://mp.weixin.qq.com/s/uOubir_nLzQtp_fWHL73JA) 中。

关于 `Map` 和 `Set`，是两个抽象数据结构（接口），`Map` 存储一个键值对集合，其中键不重复，`Set` 存储一个不重复的元素集合。

常见的 `Map` 和 `Set` 的底层实现原理有哈希表和二叉搜索树两种，比如 Java 的 HashMap/HashSet 和 C++ 的 unorderd\_map/unordered\_set 底层就是用哈希表实现，而 Java 的 TreeMap/TreeSet 和 C++ 的 map/set 底层使用红黑树这种自平衡 BST 实现的。

而本文实现的 TrieSet/TrieMap 底层则用 Trie 树这种结构来实现。

了解数据结构的读者应该知道，本质上 `Set` 可以视为一种特殊的 `Map`，`Set` 其实就是 `Map` 中的键。

**所以本文先实现 `TrieMap`，然后在 `TrieMap` 的基础上封装出 `TrieSet`**。

> PS：为了模板通用性的考虑，后文会用到 Java 的泛型，也就是用尖括号 `<>` 指定的类型变量。这些类型变量的作用是指定我们实现的容器中存储的数据类型，类比 Java 标准库的那些容器的用法应该不难理解。

前文 [学习数据结构的框架思维](https://labuladong.gitee.io/algo/1/2/) 说过，各种乱七八糟的结构都是为了在「特定场景」下尽可能高效地进行增删查改。

你比如 `HashMap<K, V>` 的优势是能够在 O(1) 时间通过键查找对应的值，但要求键的类型 `K` 必须是「可哈希」的；而 `TreeMap<K, V>` 的特点是方便根据键的大小进行操作，但要求键的类型 `K` 必须是「可比较」的。

本文要实现的 `TrieMap` 也是类似的，由于 Trie 树原理，我们要求 `TrieMap<V>` 的键必须是字符串类型，值的类型 `V` 可以随意。

接下来我们了解一下 Trie 树的原理，看看为什么这种数据结构能够高效操作字符串。

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

应合作方要求，本文不便在此发布，请扫码关注回复关键词「trie」或 [点这里](https://appktavsiei5995.pc.xiaoe-tech.com/detail/i_627cd047e4b0812e17989e70/1) 查看：

[![](https://labuladong.gitee.io/algo/images/qrcode.jpg)](https://labuladong.gitee.io/algo/images/qrcode.jpg)