---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:33:10
tags: [learn/program/algorithm/labuladong/核心框架]
title: 一个方法团灭 LeetCode 打家劫舍问题  labuladong的算法小抄
---
# 一个方法团灭 LeetCode 打家劫舍问题 Labuladong的算法小抄
-   -   [打家劫舍 I](https://labuladong.gitee.io/algo/1/14/#%E6%89%93%E5%AE%B6%E5%8A%AB%E8%88%8D-i)

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

读完本文，你不仅学会了算法套路，还可以顺便解决如下题目：

| LeetCode | 力扣 | 难度 |
| --- | --- | --- |
| [198\. House Robber](https://leetcode.com/problems/house-robber/) | [198\. 打家劫舍](https://leetcode.cn/problems/house-robber/) | 🟠 |
| [213\. House Robber II](https://leetcode.com/problems/house-robber-ii/) | [213\. 打家劫舍 II](https://leetcode.cn/problems/house-robber-ii/) | 🟠 |
| [337\. House Robber III](https://leetcode.com/problems/house-robber-iii/) | [337\. 打家劫舍 III](https://leetcode.cn/problems/house-robber-iii/) | 🟠 |
| \- | [剑指 Offer II 089. 房屋偷盗](https://leetcode.cn/problems/Gu0c2T/) | 🟠 |
| \- | [剑指 Offer II 090. 环形房屋偷盗](https://leetcode.cn/problems/PzWKhm/) | 🟠 |

**———–**

有读者私下问我 LeetCode 「打家劫舍」系列问题（英文版叫 House Robber）怎么做，我发现这一系列题目的点赞非常之高，是比较有代表性和技巧性的动态规划题目，今天就来聊聊这道题目。

打家劫舍系列总共有三道，难度设计非常合理，层层递进。第一道是比较标准的动态规划问题，而第二道融入了环形数组的条件，第三道更绝，把动态规划的自底向上和自顶向下解法和二叉树结合起来，我认为很有启发性。如果没做过的朋友，建议学习一下。

下面，我们从第一道开始分析。

## 打家劫舍 I

力扣第 198 题「 [打家劫舍](https://leetcode.cn/problems/house-robber/)」的题目如下：

街上有一排房屋，用一个包含非负整数的数组 `nums` 表示，每个元素 `nums[i]` 代表第 `i` 间房子中的现金数额。现在你是一名专业小偷，你希望**尽可能多**的盗窃这些房子中的现金，但是，**相邻的房子不能被同时盗窃**，否则会触发报警器，你就凉凉了。

请你写一个算法，计算在不触动报警器的前提下，最多能够盗窃多少现金呢？函数签名如下：

比如说输入 `nums=[2,1,7,9,3,1]`，算法返回 12，小偷可以盗窃 `nums[0], nums[3], nums[5]` 三个房屋，得到的现金之和为 2 + 9 + 1 = 12，是最优的选择。

题目很容易理解，而且动态规划的特征很明显。我们前文 [动态规划详解](https://labuladong.gitee.io/algo/3/25/69/) 做过总结，**解决动态规划问题就是找「状态」和「选择」，仅此而已**。

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

应合作方要求，本文不便在此发布，请扫码关注回复关键词「抢房子」或 [点这里](https://appktavsiei5995.pc.xiaoe-tech.com/detail/i_62987952e4b09dda12708bf8/1) 查看：

[![](https://labuladong.gitee.io/algo/images/qrcode.jpg)](https://labuladong.gitee.io/algo/images/qrcode.jpg)

**共同维护高质量学习环境，评论礼仪[见这里](https://mp.weixin.qq.com/s/YdSoYZS0QjZpbphQlpHyyA)，违者直接拉黑不解释**