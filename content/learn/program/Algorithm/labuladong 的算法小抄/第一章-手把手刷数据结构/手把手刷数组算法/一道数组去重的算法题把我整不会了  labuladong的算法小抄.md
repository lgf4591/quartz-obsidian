---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:23:36
tags: [learn/program/algorithm/labuladong/数据结构/数组]
title: 一道数组去重的算法题把我整不会了  labuladong的算法小抄
---
# 一道数组去重的算法题把我整不会了 Labuladong的算法小抄
## 一道数组去重的算法题把我整不会了

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

读完本文，你不仅学会了算法套路，还可以顺便解决如下题目：

| LeetCode | 力扣 | 难度 |
| --- | --- | --- |
| [1081\. Smallest Subsequence of Distinct Characters](https://leetcode.com/problems/smallest-subsequence-of-distinct-characters/) | [1081\. 不同字符的最小子序列](https://leetcode.cn/problems/smallest-subsequence-of-distinct-characters/) | 🟠 |
| [316\. Remove Duplicate Letters](https://leetcode.com/problems/remove-duplicate-letters/) | [316\. 去除重复字母](https://leetcode.cn/problems/remove-duplicate-letters/) | 🟠 |

**———–**

关于去重算法，应该没什么难度，往哈希集合里面塞不就行了么？

最多给你加点限制，问你怎么给有序数组原地去重，这个我们前文 [双指针技巧秒杀七道数组题目](https://labuladong.gitee.io/algo/2/20/23/) 讲过。

本文讲的问题应该是去重相关算法中难度最大的了，把这个问题搞懂，就再也不用怕数组去重问题了。

这是力扣第 316 题「 [去除重复字母](https://leetcode.cn/problems/remove-duplicate-letters/)」，题目如下：

[![](https://labuladong.gitee.io/algo/images/%e5%8d%95%e8%b0%83%e6%a0%88%e5%8e%bb%e9%87%8d/title.png)](https://labuladong.gitee.io/algo/images/%e5%8d%95%e8%b0%83%e6%a0%88%e5%8e%bb%e9%87%8d/title.png)

这道题和第 1081 题「不同字符的最小子序列」的解法是完全相同的，你可以把这道题的解法代码直接粘过去把 1081 题也干掉。

题目的要求总结出来有三点：

要求一、**要去重**。

要求二、去重字符串中的字符顺序**不能打乱 `s` 中字符出现的相对顺序**。

要求三、在所有符合上一条要求的去重字符串中，**字典序最小**的作为最终结果。

上述三条要求中，要求三可能有点难理解，举个例子。

比如说输入字符串 `s = "babc"`，去重且符合相对位置的字符串有两个，分别是 `"bac"` 和 `"abc"`，但是我们的算法得返回 `"abc"`，因为它的字典序更小。

按理说，如果我们想要有序的结果，那就得对原字符串排序对吧，但是排序后就不能保证符合 `s` 中字符出现顺序了，这似乎是矛盾的。

其实这里会借鉴后文 [单调栈解题框架](https://labuladong.gitee.io/algo/2/23/63/) 中讲到的「单调栈」的思路，没看过也无妨，等会你就明白了。

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

应合作方要求，本文不便在此发布，请扫码关注回复关键词「单调栈」或 [点这里](https://appktavsiei5995.pc.xiaoe-tech.com/detail/i_6298796ee4b0812e17a1c2fe/1) 查看：

[![](https://labuladong.gitee.io/algo/images/qrcode.jpg)](https://labuladong.gitee.io/algo/images/qrcode.jpg)

**共同维护高质量学习环境，评论礼仪[见这里](https://mp.weixin.qq.com/s/YdSoYZS0QjZpbphQlpHyyA)，违者直接拉黑不解释**