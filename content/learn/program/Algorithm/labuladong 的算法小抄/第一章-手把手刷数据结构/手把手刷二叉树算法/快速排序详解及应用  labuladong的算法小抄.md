---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:22:30
tags: [learn/program/algorithm/labuladong/数据结构/二叉树]
title: 快速排序详解及应用  labuladong的算法小抄
---
# 快速排序详解及应用 Labuladong的算法小抄
-   -   [快速排序算法思路](https://labuladong.gitee.io/algo/2/21/45/#%E5%BF%AB%E9%80%9F%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95%E6%80%9D%E8%B7%AF)

## 快速排序详解及应用

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

读完本文，你不仅学会了算法套路，还可以顺便解决如下题目：

| LeetCode | 力扣 | 难度 |
| --- | --- | --- |
| [215\. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/) | [215\. 数组中的第K个最大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/) | 🟠 |
| [912\. Sort an Array](https://leetcode.com/problems/sort-an-array/) | [912\. 排序数组](https://leetcode.cn/problems/sort-an-array/) | 🟠 |
| \- | [剑指 Offer II 076. 数组中的第 k 大的数字](https://leetcode.cn/problems/xx4gT2/) | 🟠 |

**———–**

前文 [归并排序算法详解](https://labuladong.gitee.io/algo/2/21/41/) 通过二叉树的视角描述了归并排序的算法原理以及应用，很多读者大呼精妙，那我就趁热打铁，**今天继续用二叉树的视角讲一讲快速排序算法的原理以及运用**。

### 快速排序算法思路

首先我们看一下快速排序的代码框架：

```
void sort(int[] nums, int lo, int hi) {
    if (lo >= hi) {
        return;
    }
    // 对 nums[lo..hi] 进行切分
    // 使得 nums[lo..p-1] <= nums[p] < nums[p+1..hi]
    int p = partition(nums, lo, hi);
    // 去左右子数组进行切分
    sort(nums, lo, p - 1);
    sort(nums, p + 1, hi);
}
```

其实你对比之后可以发现，快速排序就是一个二叉树的前序遍历：

```
/* 二叉树遍历框架 */
void traverse(TreeNode root) {
    if (root == null) {
        return;
    }
    /****** 前序位置 ******/
    print(root.val);
    /*********************/
    traverse(root.left);
    traverse(root.right);
}
```

另外，前文 [归并排序详解](https://labuladong.gitee.io/algo/2/21/41/) 用一句话总结了归并排序：先把左半边数组排好序，再把右半边数组排好序，然后把两半数组合并。

同时我提了一个问题，让你一句话总结快速排序，这里说一下我的答案：

**快速排序是先将一个元素排好序，然后再将剩下的元素排好序**。

为什么这么说呢，且听我慢慢道来。

快速排序的核心无疑是 `partition` 函数， `partition` 函数的作用是在 `nums[lo..hi]` 中寻找一个分界点 `p`，通过交换元素使得 `nums[lo..p-1]` 都小于等于 `nums[p]`，且 `nums[p+1..hi]` 都大于 `nums[p]`：

[![](https://labuladong.gitee.io/algo/images/%e5%bf%ab%e9%80%9f%e9%80%89%e6%8b%a9/1.jpeg)](https://labuladong.gitee.io/algo/images/%e5%bf%ab%e9%80%9f%e9%80%89%e6%8b%a9/1.jpeg)

一个元素左边的元素都比它小，右边的元素都比它大，啥意思？不就是它自己已经被放到正确的位置上了吗？

所以 `partition` 函数干的事情，其实就是把 `nums[p]` 这个元素排好序了。

一个元素被排好序了，然后呢？你再把剩下的元素排好序不就得了。

剩下的元素有哪些？左边一坨，右边一坨，去吧，对子数组进行递归，用 `partition` 函数把剩下的元素也排好序。

**从二叉树的视角，我们可以把子数组 `nums[lo..hi]` 理解成二叉树节点上的值，`sort` 函数理解成二叉树的遍历函数**。

参照二叉树的前序遍历顺序，快速排序的运行过程如下 GIF：

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

应合作方要求，本文不便在此发布，请扫码关注回复关键词「快排」或 [点这里](https://appktavsiei5995.pc.xiaoe-tech.com/detail/i_62987955e4b01a4852072fa3/1) 查看：

[![](https://labuladong.gitee.io/algo/images/qrcode.jpg)](https://labuladong.gitee.io/algo/images/qrcode.jpg)

**共同维护高质量学习环境，评论礼仪[见这里](https://mp.weixin.qq.com/s/YdSoYZS0QjZpbphQlpHyyA)，违者直接拉黑不解释**