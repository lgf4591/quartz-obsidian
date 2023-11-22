---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:30:51
tags: [learn/program/algorithm/labuladong/动态规划/游戏]
title: 经典动态规划：戳气球  labuladong的算法小抄
---
# 经典动态规划：戳气球 Labuladong的算法小抄
×

由于本站访问压力较大  
微信扫码关注公众号 **labuladong**  
回复关键词「**解锁**」  
按照操作即可解锁本站全部文章  

[![公众号](https://labuladong.gitee.io/algo/images/qrcode.jpg)](https://labuladong.gitee.io/algo/images/qrcode.jpg)

## 经典动态规划：戳气球

[![GitHub](https://img.shields.io/github/stars/labuladong/fucking-algorithm?label=Stars&style=flat&logo=GitHub)](https://github.com/labuladong/fucking-algorithm)[![](https://img.shields.io/badge/B%E7%AB%99-@labuladong-000000.svg?style=flat&logo=Bilibili)](https://space.bilibili.com/14089380)[![](https://img.shields.io/static/v1?label=%E9%85%8D%E5%A5%97PDF%E5%92%8C%E6%8F%92%E4%BB%B6&message=%E4%B8%8B%E8%BD%BD&color=red&style=flat)](https://mp.weixin.qq.com/s/X-fE9sR4BLi6T9pn7xP4pg)[![](https://img.shields.io/static/v1?label=%E6%89%93%E5%8D%A1%E6%8C%91%E6%88%98&message=%E6%8A%A5%E5%90%8D&color=green&style=flat)](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q)[![](https://img.shields.io/static/v1?label=%E7%B2%BE%E5%93%81%E8%AF%BE%E7%A8%8B&message=%E6%9F%A5%E7%9C%8B&color=pink&style=flat)](https://appktavsiei5995.pc.xiaoe-tech.com/index)

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

读完本文，你不仅学会了算法套路，还可以顺便解决如下题目：

| LeetCode | 力扣 | 难度 |
| --- | --- | --- |
| [312\. Burst Balloons](https://leetcode.com/problems/burst-balloons/) | [312\. 戳气球](https://leetcode.cn/problems/burst-balloons/) | 🔴 |

**———–**

今天我们要聊的这道题「Burst Balloon」和之前我们写过的那篇 [经典动态规划：高楼扔鸡蛋问题](https://labuladong.gitee.io/algo/3/28/91/) 分析过的高楼扔鸡蛋问题类似，知名度很高，但难度确实也很大。因此我的公众号就给这道题赐个座，来看一看这道题目到底有多难。

它是力扣第 312 题「 [戳气球](https://leetcode.cn/problems/burst-balloons/)」，题目如下：

[![](https://labuladong.gitee.io/algo/images/burstBalloon/title.jpg)](https://labuladong.gitee.io/algo/images/burstBalloon/title.jpg)

首先必须要说明，这个题目的状态转移方程真的比较巧妙，所以说如果你看了题目之后完全没有思路恰恰是正常的。虽然最优答案不容易想出来，但基本的思路分析是我们应该力求做到的。所以本文会先分析一下常规思路，然后再引入动态规划解法。

### 一、回溯思路

先来顺一下解决这种问题的套路：

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

应合作方要求，本文不便在此发布，请扫码关注回复关键词「气球」或 [点这里](https://appktavsiei5995.pc.xiaoe-tech.com/detail/i_62987946e4b01a4852072f91/1) 查看：

[![](https://labuladong.gitee.io/algo/images/qrcode.jpg)](https://labuladong.gitee.io/algo/images/qrcode.jpg)