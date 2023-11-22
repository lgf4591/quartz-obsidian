---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:21:34
tags: [learn/program/algorithm/labuladong/数据结构/二叉树]
title: 东哥带你刷二叉树（后序篇）  labuladong的算法小抄
---
# 东哥带你刷二叉树（后序篇） Labuladong的算法小抄
## 东哥带你刷二叉树（后序篇）

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

读完本文，你不仅学会了算法套路，还可以顺便解决如下题目：

| LeetCode | 力扣 | 难度 |
| --- | --- | --- |
| [652\. Find Duplicate Subtrees](https://leetcode.com/problems/find-duplicate-subtrees/) | [652\. 寻找重复的子树](https://leetcode.cn/problems/find-duplicate-subtrees/) | 🟠 |

**———–**

PS： [刷题插件](https://mp.weixin.qq.com/s/OE1zPVPj0V2o82N4HtLQbw) 集成了手把手刷二叉树功能，按照公式和套路讲解了 150 道二叉树题目，可手把手带你刷完二叉树分类的题目，迅速掌握递归思维。

本文是承接 [东哥带你刷二叉树（纲领篇）](https://labuladong.gitee.io/algo/2/21/36/) 的第四篇文章，主要讲二叉树后序位置的妙用，复述下前文关于后序遍历的描述：

> 前序位置的代码只能从函数参数中获取父节点传递来的数据，而后序位置的代码不仅可以获取参数数据，还可以获取到子树通过函数返回值传递回来的数据。
>
> **那么换句话说，一旦你发现题目和子树有关，那大概率要给函数设置合理的定义和返回值，在后序位置写代码了**。

多说无益，我们直接看题，这是力扣第 652 题「 [寻找重复的子树](https://leetcode.cn/problems/find-duplicate-subtrees/)」：

[![](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%8f%89%e6%a0%913/title.png)](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%8f%89%e6%a0%913/title.png)

函数签名如下：

```
List<TreeNode> findDuplicateSubtrees(TreeNode root);
```

我来简单解释下题目，输入是一棵二叉树的根节点 `root`，返回的是一个列表，里面装着若干个二叉树节点，这些节点对应的子树在原二叉树中是存在重复的。

说起来比较绕，举例来说，比如输入如下的二叉树：

[![](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%8f%89%e6%a0%913/1.png)](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%8f%89%e6%a0%913/1.png)

首先，节点 4 本身可以作为一棵子树，且二叉树中有多个节点 4：

[![](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%8f%89%e6%a0%913/2.png)](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%8f%89%e6%a0%913/2.png)

类似的，还存在两棵以 2 为根的重复子树：

[![](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%8f%89%e6%a0%913/3.png)](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%8f%89%e6%a0%913/3.png)

那么，我们返回的 `List` 中就应该有两个 `TreeNode`，值分别为 4 和 2（具体是哪个节点都无所谓）。

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

应合作方要求，本文不便在此发布，请扫码关注回复关键词「二叉树」或 [点这里](https://appktavsiei5995.pc.xiaoe-tech.com/detail/i_62987964e4b09dda12708c0b/1) 查看：

[![](https://labuladong.gitee.io/algo/images/qrcode.jpg)](https://labuladong.gitee.io/algo/images/qrcode.jpg)

**共同维护高质量学习环境，评论礼仪[见这里](https://mp.weixin.qq.com/s/YdSoYZS0QjZpbphQlpHyyA)，违者直接拉黑不解释**