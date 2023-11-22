---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:29:35
tags: [learn/program/algorithm/labuladong/动态规划/子序列问题]
title: 动态规划之子序列问题解题模板  labuladong的算法小抄
---
# 动态规划之子序列问题解题模板 Labuladong的算法小抄
×

由于本站访问压力较大  
微信扫码关注公众号 **labuladong**  
回复关键词「**解锁**」  
按照操作即可解锁本站全部文章  

[![公众号](https://labuladong.gitee.io/algo/images/qrcode.jpg)](https://labuladong.gitee.io/algo/images/qrcode.jpg)

## 动态规划之子序列问题解题模板

[![GitHub](https://img.shields.io/github/stars/labuladong/fucking-algorithm?label=Stars&style=flat&logo=GitHub)](https://github.com/labuladong/fucking-algorithm)[![](https://img.shields.io/badge/B%E7%AB%99-@labuladong-000000.svg?style=flat&logo=Bilibili)](https://space.bilibili.com/14089380)[![](https://img.shields.io/static/v1?label=%E9%85%8D%E5%A5%97PDF%E5%92%8C%E6%8F%92%E4%BB%B6&message=%E4%B8%8B%E8%BD%BD&color=red&style=flat)](https://mp.weixin.qq.com/s/X-fE9sR4BLi6T9pn7xP4pg)[![](https://img.shields.io/static/v1?label=%E6%89%93%E5%8D%A1%E6%8C%91%E6%88%98&message=%E6%8A%A5%E5%90%8D&color=green&style=flat)](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q)[![](https://img.shields.io/static/v1?label=%E7%B2%BE%E5%93%81%E8%AF%BE%E7%A8%8B&message=%E6%9F%A5%E7%9C%8B&color=pink&style=flat)](https://appktavsiei5995.pc.xiaoe-tech.com/index)

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

读完本文，你不仅学会了算法套路，还可以顺便解决如下题目：

| LeetCode | 力扣 | 难度 |
| --- | --- | --- |
| [1312\. Minimum Insertion Steps to Make a String Palindrome](https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/) | [1312\. 让字符串成为回文串的最少插入次数](https://leetcode.cn/problems/minimum-insertion-steps-to-make-a-string-palindrome/) | 🔴 |
| [516\. Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/) | [516\. 最长回文子序列](https://leetcode.cn/problems/longest-palindromic-subsequence/) | 🟠 |

**———–**

子序列问题是常见的算法问题，而且并不好解决。

首先，子序列问题本身就相对子串、子数组更困难一些，因为前者是不连续的序列，而后两者是连续的，就算穷举你都不一定会，更别说求解相关的算法问题了。

而且，子序列问题很可能涉及到两个字符串，比如前文 [最长公共子序列](https://labuladong.gitee.io/algo/3/26/78/)，如果没有一定的处理经验，真的不容易想出来。所以本文就来扒一扒子序列问题的套路，其实就有两种模板，相关问题只要往这两种思路上想，十拿九稳。

一般来说，这类问题都是让你求一个**最长子序列**，因为最短子序列就是一个字符嘛，没啥可问的。一旦涉及到子序列和最值，那几乎可以肯定，**考察的是动态规划技巧，时间复杂度一般都是 O(n^2)**。

原因很简单，你想想一个字符串，它的子序列有多少种可能？起码是指数级的吧，这种情况下，不用动态规划技巧，还想怎么着？

既然要用动态规划，那就要定义 `dp` 数组，找状态转移关系。我们说的两种思路模板，就是 `dp` 数组的定义思路。不同的问题可能需要不同的 `dp` 数组定义来解决。

### 一、两种思路

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

应合作方要求，本文不便在此发布，请扫码关注回复关键词「子序列」或 [点这里](https://appktavsiei5995.pc.xiaoe-tech.com/detail/i_62987943e4b01c509ab8b6aa/1) 查看：

[![](https://labuladong.gitee.io/algo/images/qrcode.jpg)](https://labuladong.gitee.io/algo/images/qrcode.jpg)