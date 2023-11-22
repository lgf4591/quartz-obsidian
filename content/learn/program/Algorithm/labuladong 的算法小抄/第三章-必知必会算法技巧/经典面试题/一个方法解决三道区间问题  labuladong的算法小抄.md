---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:27:50
tags: [learn/program/algorithm/labuladong/技巧/面试]
title: 一个方法解决三道区间问题 Labuladong的算法小抄
---
# 一个方法解决三道区间问题 Labuladong的算法小抄
## 一个方法解决三道区间问题

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

读完本文，你不仅学会了算法套路，还可以顺便解决如下题目：

| LeetCode | 力扣 | 难度 |
| --- | --- | --- |
| [1288\. Remove Covered Intervals](https://leetcode.com/problems/remove-covered-intervals/) | [1288\. 删除被覆盖区间](https://leetcode.cn/problems/remove-covered-intervals/) | 🟠 |
| [56\. Merge Intervals](https://leetcode.com/problems/merge-intervals/) | [56\. 合并区间](https://leetcode.cn/problems/merge-intervals/) | 🟠 |
| [986\. Interval List Intersections](https://leetcode.com/problems/interval-list-intersections/) | [986\. 区间列表的交集](https://leetcode.cn/problems/interval-list-intersections/) | 🟠 |
| \- | [剑指 Offer II 074. 合并区间](https://leetcode.cn/problems/SsGoHC/) | 🟠 |

**———–**

经常有读者问区间相关的问题，今天写一篇文章，秒杀三道区间相关的问题。

所谓区间问题，就是线段问题，让你合并所有线段、找出线段的交集等等。主要有两个技巧：

**1、排序**。常见的排序方法就是按照区间起点排序，或者先按照起点升序排序，若起点相同，则按照终点降序排序。当然，如果你非要按照终点排序，无非对称操作，本质都是一样的。

**2、画图**。就是说不要偷懒，勤动手，两个区间的相对位置到底有几种可能，不同的相对位置我们的代码应该怎么去处理。

废话不多说，下面我们来做题。

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

应合作方要求，本文不便在此发布，请扫码关注回复关键词「区间」或 [点这里](https://appktavsiei5995.pc.xiaoe-tech.com/detail/i_6298794fe4b01a4852072f9b/1) 查看：

[![](https://labuladong.gitee.io/algo/images/qrcode.jpg)](https://labuladong.gitee.io/algo/images/qrcode.jpg)