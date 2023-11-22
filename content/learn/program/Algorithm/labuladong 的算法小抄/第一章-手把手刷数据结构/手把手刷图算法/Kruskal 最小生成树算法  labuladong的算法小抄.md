---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:22:54
tags: [learn/program/algorithm/labuladong/数据结构/图]
title: Kruskal 最小生成树算法  labuladong的算法小抄
---
# Kruskal 最小生成树算法 Labuladong的算法小抄
-   -   [什么是最小生成树](https://labuladong.gitee.io/algo/2/22/54/#%E4%BB%80%E4%B9%88%E6%98%AF%E6%9C%80%E5%B0%8F%E7%94%9F%E6%88%90%E6%A0%91)

×

由于本站访问压力较大  
微信扫码关注公众号 **labuladong**  
回复关键词「**解锁**」  
按照操作即可解锁本站全部文章  

[![公众号](https://labuladong.gitee.io/algo/images/qrcode.jpg)](https://labuladong.gitee.io/algo/images/qrcode.jpg)

[![GitHub](https://img.shields.io/github/stars/labuladong/fucking-algorithm?label=Stars&style=flat&logo=GitHub)](https://github.com/labuladong/fucking-algorithm)[![](https://img.shields.io/badge/B%E7%AB%99-@labuladong-000000.svg?style=flat&logo=Bilibili)](https://space.bilibili.com/14089380)[![](https://img.shields.io/static/v1?label=%E9%85%8D%E5%A5%97PDF%E5%92%8C%E6%8F%92%E4%BB%B6&message=%E4%B8%8B%E8%BD%BD&color=red&style=flat)](https://mp.weixin.qq.com/s/X-fE9sR4BLi6T9pn7xP4pg)[![](https://img.shields.io/static/v1?label=%E6%89%93%E5%8D%A1%E6%8C%91%E6%88%98&message=%E6%8A%A5%E5%90%8D&color=green&style=flat)](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q)[![](https://img.shields.io/static/v1?label=%E7%B2%BE%E5%93%81%E8%AF%BE%E7%A8%8B&message=%E6%9F%A5%E7%9C%8B&color=pink&style=flat)](https://appktavsiei5995.pc.xiaoe-tech.com/index)

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

读完本文，你不仅学会了算法套路，还可以顺便解决如下题目：

| LeetCode | 力扣 | 难度 |
| --- | --- | --- |
| [1135\. Connecting Cities With Minimum Cost](https://leetcode.com/problems/connecting-cities-with-minimum-cost/)🔒 | [1135\. 最低成本联通所有城市](https://leetcode.cn/problems/connecting-cities-with-minimum-cost/)🔒 | 🟠 |
| [1584\. Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/) | [1584\. 连接所有点的最小费用](https://leetcode.cn/problems/min-cost-to-connect-all-points/) | 🟠 |
| [261\. Graph Valid Tree](https://leetcode.com/problems/graph-valid-tree/)🔒 | [261\. 以图判树](https://leetcode.cn/problems/graph-valid-tree/)🔒 | 🟠 |

**———–**

图论中知名度比较高的算法应该就是 [Dijkstra 最短路径算法](https://labuladong.gitee.io/algo/2/22/56/)， [环检测和拓扑排序](https://labuladong.gitee.io/algo/2/22/51/)， [二分图判定算法](https://labuladong.gitee.io/algo/2/22/52/) 以及今天要讲的最小生成树（Minimum Spanning Tree）算法了。

最小生成树算法主要有 Prim 算法（普里姆算法）和 Kruskal 算法（克鲁斯卡尔算法）两种，这两种算法虽然都运用了贪心思想，但从实现上来说差异还是蛮大的。

本文先来讲比较简单易懂的 Kruskal 算法，然后在下一篇文章 [Prim 算法模板](https://labuladong.gitee.io/algo/2/22/55/) 中聊 Prim 算法。

Kruskal 算法其实很容易理解和记忆，其关键是要熟悉并查集算法，如果不熟悉，建议先看下前文 [Union-Find 并查集算法](https://labuladong.gitee.io/algo/2/22/53/)。

接下来，我们从最小生成树的定义说起。

## 什么是最小生成树

**先说「树」和「图」的根本区别：树不会包含环，图可以包含环**。

如果一幅图没有环，完全可以拉伸成一棵树的模样。说的专业一点，树就是「无环连通图」。

那么什么是图的「生成树」呢，其实按字面意思也好理解，就是在图中找一棵包含图中的所有节点的树。专业点说，生成树是含有图中所有顶点的「无环连通子图」。

容易想到，一幅图可以有很多不同的生成树，比如下面这幅图，红色的边就组成了两棵不同的生成树：

[![](https://labuladong.gitee.io/algo/images/kruskal/1.png)](https://labuladong.gitee.io/algo/images/kruskal/1.png)

对于加权图，每条边都有权重，所以每棵生成树都有一个权重和。比如上图，右侧生成树的权重和显然比左侧生成树的权重和要小。

**那么最小生成树很好理解了，所有可能的生成树中，权重和最小的那棵生成树就叫「最小生成树」**。

> PS：一般来说，我们都是在**无向加权图**中计算最小生成树的，所以使用最小生成树算法的现实场景中，图的边权重一般代表成本、距离这样的标量。

在讲 Kruskal 算法之前，需要回顾一下 Union-Find 并查集算法。

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

应合作方要求，本文不便在此发布，请扫码关注回复关键词「生成树」或 [点这里](https://appktavsiei5995.pc.xiaoe-tech.com/detail/i_6298793ce4b01a4852072f86/1) 查看：

[![](https://labuladong.gitee.io/algo/images/qrcode.jpg)](https://labuladong.gitee.io/algo/images/qrcode.jpg)

**共同维护高质量学习环境，评论礼仪[见这里](https://mp.weixin.qq.com/s/YdSoYZS0QjZpbphQlpHyyA)，违者直接拉黑不解释**