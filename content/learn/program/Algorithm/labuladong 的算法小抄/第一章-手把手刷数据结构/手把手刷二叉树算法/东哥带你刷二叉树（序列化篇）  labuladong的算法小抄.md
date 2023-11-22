---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:21:41
tags: [learn/program/algorithm/labuladong/数据结构/二叉树]
title: 东哥带你刷二叉树（序列化篇）  labuladong的算法小抄
---
# 东哥带你刷二叉树（序列化篇） Labuladong的算法小抄
## 东哥带你刷二叉树（序列化篇）

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

读完本文，你不仅学会了算法套路，还可以顺便解决如下题目：

| LeetCode | 力扣 | 难度 |
| --- | --- | --- |
| [297\. Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/) | [297\. 二叉树的序列化与反序列化](https://leetcode.cn/problems/serialize-and-deserialize-binary-tree/) | 🔴 |
| \- | [剑指 Offer 37. 序列化二叉树](https://leetcode.cn/problems/xu-lie-hua-er-cha-shu-lcof/) | 🔴 |
| \- | [剑指 Offer II 048. 序列化与反序列化二叉树](https://leetcode.cn/problems/h54YBf/) | 🔴 |

**———–**

PS： [刷题插件](https://mp.weixin.qq.com/s/OE1zPVPj0V2o82N4HtLQbw) 集成了手把手刷二叉树功能，按照公式和套路讲解了 150 道二叉树题目，可手把手带你刷完二叉树分类的题目，迅速掌握递归思维。

本文是承接 [东哥带你刷二叉树（纲领篇）](https://labuladong.gitee.io/algo/2/21/36/) 的第三篇文章，前文 [东哥带你刷二叉树（构造篇）](https://labuladong.gitee.io/algo/2/21/38/) 带你学习了二叉树构造技巧，本文加大难度，让你对二叉树同时进行「序列化」和「反序列化」。

要说序列化和反序列化，得先从 JSON 数据格式说起。

JSON 的运用非常广泛，比如我们经常将变成语言中的结构体序列化成 JSON 字符串，存入缓存或者通过网络发送给远端服务，消费者接受 JSON 字符串然后进行反序列化，就可以得到原始数据了。

这就是序列化和反序列化的目的，以某种特定格式组织数据，使得数据可以独立于编程语言。

那么假设现在有一棵用 Java 实现的二叉树，我想把它通过某些方式存储下来，然后用 C++ 读取这棵并还原这棵二叉树的结构，怎么办？这就需要对二叉树进行序列化和反序列化了。

直接看题吧。

### 一、题目描述

力扣第 297 题「 [二叉树的序列化与反序列化](https://leetcode.cn/problems/serialize-and-deserialize-binary-tree/)」就是给你输入一棵二叉树的根节点 `root`，要求你实现如下一个类：

```
public class Codec {

    // 把一棵二叉树序列化成字符串
    public String serialize(TreeNode root) {}

    // 把字符串反序列化成二叉树
    public TreeNode deserialize(String data) {}
}
```

我们可以用 `serialize` 方法将二叉树序列化成字符串，用 `deserialize` 方法将序列化的字符串反序列化成二叉树，至于以什么格式序列化和反序列化，这个完全由你决定。

比如说输入如下这样一棵二叉树：

[![](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%8f%89%e6%a0%91%e5%ba%8f%e5%88%97%e5%8c%96/1.jpg)](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%8f%89%e6%a0%91%e5%ba%8f%e5%88%97%e5%8c%96/1.jpg)

`serialize` 方法也许会把它序列化成字符串 `2,1,#,6,#,#,3,#,#`，其中 `#` 表示 `null` 指针，那么把这个字符串再输入 `deserialize` 方法，依然可以还原出这棵二叉树。

也就是说，这两个方法会成对儿使用，你只要保证他俩能够自洽就行了。

想象一下，二叉树结该是一个二维平面内的结构，而序列化出来的字符串是一个线性的一维结构。**所谓的序列化不过就是把结构化的数据「打平」，本质就是在考察二叉树的遍历方式**。

二叉树的遍历方式有哪些？递归遍历方式有前序遍历，中序遍历，后序遍历；迭代方式一般是层级遍历。本文就把这些方式都尝试一遍，来实现 `serialize` 方法和 `deserialize` 方法。

### 二、前序遍历解法

前文 [学习数据结构和算法的框架思维](https://labuladong.gitee.io/algo/1/2/) 说过了二叉树的几种遍历方式，前序遍历框架如下：

```
void traverse(TreeNode root) {
    if (root == null) return;

    // 前序位置的代码
    traverse(root.left);
    traverse(root.right);
}
```

真的很简单，在递归遍历两棵子树之前写的代码就是前序遍历代码，那么请你看一看如下伪码：

```
LinkedList<Integer> res;
void traverse(TreeNode root) {
    if (root == null) {
        // 暂且用数字 -1 代表空指针 null
        res.addLast(-1);
        return;
    }

    /****** 前序位置 ******/
    res.addLast(root.val);
    /***********************/

    traverse(root.left);
    traverse(root.right);
}
```

调用 `traverse` 函数之后，你是否可以立即想出这个 `res` 列表中元素的顺序是怎样的？比如如下二叉树（`#` 代表空指针 null），可以直观看出前序遍历做的事情：

[![](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%8f%89%e6%a0%91%e5%ba%8f%e5%88%97%e5%8c%96/1.jpeg)](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%8f%89%e6%a0%91%e5%ba%8f%e5%88%97%e5%8c%96/1.jpeg)

那么 `res = [1,2,-1,4,-1,-1,3,-1,-1]`，这就是将二叉树「打平」到了一个列表中，其中 -1 代表 null。

那么，将二叉树打平到一个字符串中也是完全一样的：

```
// 代表分隔符的字符
String SEP = ",";
// 代表 null 空指针的字符
String NULL = "#";
// 用于拼接字符串
StringBuilder sb = new StringBuilder();

/* 将二叉树打平为字符串 */
void traverse(TreeNode root, StringBuilder sb) {
    if (root == null) {
        sb.append(NULL).append(SEP);
        return;
    }

    /****** 前序位置 ******/
    sb.append(root.val).append(SEP);
    /***********************/

    traverse(root.left, sb);
    traverse(root.right, sb);
}
```

`StringBuilder` 可以用于高效拼接字符串，所以也可以认为是一个列表，用 `,` 作为分隔符，用 `#` 表示空指针 null，调用完 `traverse` 函数后，`sb` 中的字符串应该是 `1,2,#,4,#,#,3,#,#,`。

至此，我们已经可以写出序列化函数 `serialize` 的代码了：

```
String SEP = ",";
String NULL = "#";

/* 主函数，将二叉树序列化为字符串 */
String serialize(TreeNode root) {
    StringBuilder sb = new StringBuilder();
    serialize(root, sb);
    return sb.toString();
}

/* 辅助函数，将二叉树存入 StringBuilder */
void serialize(TreeNode root, StringBuilder sb) {
    if (root == null) {
        sb.append(NULL).append(SEP);
        return;
    }

    /****** 前序位置 ******/
    sb.append(root.val).append(SEP);


    /***********************/

    serialize(root.left, sb);
    serialize(root.right, sb);
}
```

现在，思考一下如何写 `deserialize` 函数，将字符串反过来构造二叉树。

首先我们可以把字符串转化成列表：

```
String data = "1,2,#,4,#,#,3,#,#,";
String[] nodes = data.split(",");
```

这样，`nodes` 列表就是二叉树的前序遍历结果，问题转化为：如何通过二叉树的前序遍历结果还原一棵二叉树？

> PS：前文 [东哥带你刷二叉树（构造篇）](https://labuladong.gitee.io/algo/2/21/38/)，至少要得到前、中、后序遍历中的两种互相配合才能还原二叉树，那是因为前文的遍历结果没有记录空指针的信息。这里的 `node` 列表包含了空指针的信息，所以只使用 `node` 列表就可以还原二叉树。

根据我们刚才的分析，`nodes` 列表就是一棵打平的二叉树：

[![](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%8f%89%e6%a0%91%e5%ba%8f%e5%88%97%e5%8c%96/1.jpeg)](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%8f%89%e6%a0%91%e5%ba%8f%e5%88%97%e5%8c%96/1.jpeg)

那么，反序列化过程也是一样，**先确定根节点 `root`，然后遵循前序遍历的规则，递归生成左右子树即可**：

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

应合作方要求，本文不便在此发布，请扫码关注回复关键词「序列化」或 [点这里](https://appktavsiei5995.pc.xiaoe-tech.com/detail/i_62987967e4b0812e17a1c2f7/1) 查看：

[![](https://labuladong.gitee.io/algo/images/qrcode.jpg)](https://labuladong.gitee.io/algo/images/qrcode.jpg)