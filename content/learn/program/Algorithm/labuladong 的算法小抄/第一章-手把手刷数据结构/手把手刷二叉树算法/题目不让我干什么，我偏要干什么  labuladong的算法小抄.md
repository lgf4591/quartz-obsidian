---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:22:41
tags: [learn/program/algorithm/labuladong/数据结构/二叉树]
title: 题目不让我干什么，我偏要干什么  labuladong的算法小抄
---
# 题目不让我干什么，我偏要干什么 Labuladong的算法小抄
-   -   [一、题目描述](https://labuladong.gitee.io/algo/2/21/46/#%E4%B8%80%E9%A2%98%E7%9B%AE%E6%8F%8F%E8%BF%B0)
    -   [二、解题思路](https://labuladong.gitee.io/algo/2/21/46/#%E4%BA%8C%E8%A7%A3%E9%A2%98%E6%80%9D%E8%B7%AF)

## 题目不让我干什么，我偏要干什么

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

读完本文，你不仅学会了算法套路，还可以顺便解决如下题目：

| LeetCode | 力扣 | 难度 |
| --- | --- | --- |
| [341\. Flatten Nested List Iterator](https://leetcode.com/problems/flatten-nested-list-iterator/) | [341\. 扁平化嵌套列表迭代器](https://leetcode.cn/problems/flatten-nested-list-iterator/) | 🟠 |

**———–**

PS： [刷题插件](https://mp.weixin.qq.com/s/OE1zPVPj0V2o82N4HtLQbw) 集成了手把手刷二叉树功能，按照公式和套路讲解了 150 道二叉树题目，可手把手带你刷完二叉树分类的题目，迅速掌握递归思维。

今天来讲一道非常有启发性的设计题目，为什么说它有启发性，我们后面再说。

### 一、题目描述

这是力扣第 341 题「 [扁平化嵌套列表迭代器](https://leetcode.cn/problems/flatten-nested-list-iterator/)」，我来描述一下题目：

首先，现在有一种数据结构 `NestedInteger`，**这个结构中存的数据可能是一个 `Integer` 整数，也可能是一个 `NestedInteger` 列表**。注意，这个列表里面装着的是 `NestedInteger`，也就是说这个列表中的每一个元素可能是个整数，可能又是个列表，这样无限递归嵌套下去……

`NestedInteger` 有如下 API：

```
public class NestedInteger {
    // 如果其中存的是一个整数，则返回 true，否则返回 false
    public boolean isInteger();

    // 如果其中存的是一个整数，则返回这个整数，否则返回 null
    public Integer getInteger();

    // 如果其中存的是一个列表，则返回这个列表，否则返回 null
    public List<NestedInteger> getList();
}
```

我们的算法会被输入一个 `NestedInteger` 列表，我们需要做的就是写一个迭代器类，将这个带有嵌套结构 `NestedInteger` 的列表「拍平」：

```
public class NestedIterator implements Iterator<Integer> {
    // 构造器输入一个 NestedInteger 列表
    public NestedIterator(List<NestedInteger> nestedList) {}
    
    // 返回下一个整数
    public Integer next() {}

    // 是否还有下一个元素？
    public boolean hasNext() {}
}
```

我们写的这个类会被这样调用，**先调用 `hasNext` 方法，后调用 `next` 方法**：

```
NestedIterator i = new NestedIterator(nestedList);
while (i.hasNext())
    print(i.next());
```

[![](https://labuladong.gitee.io/algo/images/nestedList/title.jpg)](https://labuladong.gitee.io/algo/images/nestedList/title.jpg)

比如示例 1，输入的列表里有三个 `NestedInteger`，两个列表型的 `NestedInteger` 和一个整数型的 `NestedInteger`。

学过设计模式的朋友应该知道，迭代器也是设计模式的一种，目的就是为调用者屏蔽底层数据结构的细节，简单地通过 `hasNext` 和 `next` 方法有序地进行遍历。

为什么说这个题目很有启发性呢？因为我最近在用一款类似印象笔记的软件，叫做 Notion（挺有名的）。这个软件的一个亮点就是「万物皆 block」，比如说标题、页面、表格都是 block。有的 block 甚至可以无限嵌套，这就打破了传统笔记本「文件夹」->「笔记本」->「笔记」的三层结构。

回想这个算法问题，`NestedInteger` 结构实际上也是一种支持无限嵌套的结构，而且可以同时表示整数和列表两种不同类型，我想 Notion 的核心数据结构 block 估计也是这样的一种设计思路。

那么话说回来，对于这个算法问题，我们怎么解决呢？`NestedInteger` 结构可以无限嵌套，怎么把这个结构「打平」，为迭代器的调用者屏蔽底层细节，得到扁平化的输出呢？

### 二、解题思路

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

应合作方要求，本文不便在此发布，请扫码关注回复关键词「迭代器」或 [点这里](https://appktavsiei5995.pc.xiaoe-tech.com/detail/i_62987934e4b0cedf38ba0686/1) 查看：

[![](https://labuladong.gitee.io/algo/images/qrcode.jpg)](https://labuladong.gitee.io/algo/images/qrcode.jpg)

**共同维护高质量学习环境，评论礼仪[见这里](https://mp.weixin.qq.com/s/YdSoYZS0QjZpbphQlpHyyA)，违者直接拉黑不解释**