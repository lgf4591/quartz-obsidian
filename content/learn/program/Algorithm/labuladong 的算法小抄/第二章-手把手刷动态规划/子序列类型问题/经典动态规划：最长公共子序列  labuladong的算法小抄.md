---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:29:55
tags: [learn/program/algorithm/labuladong/动态规划/子序列问题]
title: 经典动态规划：最长公共子序列  labuladong的算法小抄
---
# 经典动态规划：最长公共子序列 Labuladong的算法小抄
×

由于本站访问压力较大  
微信扫码关注公众号 **labuladong**  
回复关键词「**解锁**」  
按照操作即可解锁本站全部文章  

[![公众号](https://labuladong.gitee.io/algo/images/qrcode.jpg)](https://labuladong.gitee.io/algo/images/qrcode.jpg)

## 经典动态规划：最长公共子序列

[![GitHub](https://img.shields.io/github/stars/labuladong/fucking-algorithm?label=Stars&style=flat&logo=GitHub)](https://github.com/labuladong/fucking-algorithm)[![](https://img.shields.io/badge/B%E7%AB%99-@labuladong-000000.svg?style=flat&logo=Bilibili)](https://space.bilibili.com/14089380)[![](https://img.shields.io/static/v1?label=%E9%85%8D%E5%A5%97PDF%E5%92%8C%E6%8F%92%E4%BB%B6&message=%E4%B8%8B%E8%BD%BD&color=red&style=flat)](https://mp.weixin.qq.com/s/X-fE9sR4BLi6T9pn7xP4pg)[![](https://img.shields.io/static/v1?label=%E6%89%93%E5%8D%A1%E6%8C%91%E6%88%98&message=%E6%8A%A5%E5%90%8D&color=green&style=flat)](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q)[![](https://img.shields.io/static/v1?label=%E7%B2%BE%E5%93%81%E8%AF%BE%E7%A8%8B&message=%E6%9F%A5%E7%9C%8B&color=pink&style=flat)](https://appktavsiei5995.pc.xiaoe-tech.com/index)

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

读完本文，你不仅学会了算法套路，还可以顺便解决如下题目：

| LeetCode | 力扣 | 难度 |
| --- | --- | --- |
| [1143\. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/) | [1143\. 最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/) | 🟠 |
| [583\. Delete Operation for Two Strings](https://leetcode.com/problems/delete-operation-for-two-strings/) | [583\. 两个字符串的删除操作](https://leetcode.cn/problems/delete-operation-for-two-strings/) | 🟠 |
| [712\. Minimum ASCII Delete Sum for Two Strings](https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings/) | [712\. 两个字符串的最小ASCII删除和](https://leetcode.cn/problems/minimum-ascii-delete-sum-for-two-strings/) | 🟠 |
| \- | [剑指 Offer II 095. 最长公共子序列](https://leetcode.cn/problems/qJnOS7/) | 🟠 |

**———–**

不知道大家做算法题有什么感觉，**我总结出来做算法题的技巧就是，把大的问题细化到一个点，先研究在这个小的点上如何解决问题，然后再通过递归/迭代的方式扩展到整个问题**。

比如说我们前文 [手把手带你刷二叉树第三期](https://labuladong.gitee.io/algo/2/21/40/)，解决二叉树的题目，我们就会把整个问题细化到某一个节点上，想象自己站在某个节点上，需要做什么，然后套二叉树递归框架就行了。

动态规划系列问题也是一样，尤其是子序列相关的问题。**本文从「最长公共子序列问题」展开，总结三道子序列问题**，解这道题仔细讲讲这种子序列问题的套路，你就能感受到这种思维方式了。

### 最长公共子序列

计算最长公共子序列（Longest Common Subsequence，简称 LCS）是一道经典的动态规划题目，力扣第 1143 题「 [最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/)」就是这个问题：

给你输入两个字符串 `s1` 和 `s2`，请你找出他们俩的最长公共子序列，返回这个子序列的长度。函数签名如下：

```
int longestCommonSubsequence(String s1, String s2);
```

比如说输入 `s1 = "zabcde", s2 = "acez"`，它俩的最长公共子序列是 `lcs = "ace"`，长度为 3，所以算法返回 3。

如果没有做过这道题，一个最简单的暴力算法就是，把 `s1` 和 `s2` 的所有子序列都穷举出来，然后看看有没有公共的，然后在所有公共子序列里面再寻找一个长度最大的。

显然，这种思路的复杂度非常高，你要穷举出所有子序列，这个复杂度就是指数级的，肯定不实际。

正确的思路是不要考虑整个字符串，而是细化到 `s1` 和 `s2` 的每个字符。后文 [子序列解题模板](https://labuladong.gitee.io/algo/3/26/79/) 中总结的一个规律：

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

应合作方要求，本文不便在此发布，请扫码关注回复关键词「LCS」或 [点这里](https://appktavsiei5995.pc.xiaoe-tech.com/detail/i_6298793ae4b09dda12708be8/1) 查看：

[![](https://labuladong.gitee.io/algo/images/qrcode.jpg)](https://labuladong.gitee.io/algo/images/qrcode.jpg)