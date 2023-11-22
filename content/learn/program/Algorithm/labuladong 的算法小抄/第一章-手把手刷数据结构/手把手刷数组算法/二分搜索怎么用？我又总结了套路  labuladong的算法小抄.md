---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:23:41
tags: [learn/program/algorithm/labuladong/数据结构/数组]
title: 二分搜索怎么用？我又总结了套路  labuladong的算法小抄
---
# 二分搜索怎么用？我又总结了套路 Labuladong的算法小抄
-   -   [原始的二分搜索代码](https://labuladong.gitee.io/algo/2/20/31/#%E5%8E%9F%E5%A7%8B%E7%9A%84%E4%BA%8C%E5%88%86%E6%90%9C%E7%B4%A2%E4%BB%A3%E7%A0%81)

## 二分搜索怎么用？我又总结了套路

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

读完本文，你不仅学会了算法套路，还可以顺便解决如下题目：

| LeetCode | 力扣 | 难度 |
| --- | --- | --- |
| [1011\. Capacity To Ship Packages Within D Days](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/) | [1011\. 在 D 天内送达包裹的能力](https://leetcode.cn/problems/capacity-to-ship-packages-within-d-days/) | 🟠 |
| [410\. Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/) | [410\. 分割数组的最大值](https://leetcode.cn/problems/split-array-largest-sum/) | 🔴 |
| [875\. Koko Eating Bananas](https://leetcode.com/problems/koko-eating-bananas/) | [875\. 爱吃香蕉的珂珂](https://leetcode.cn/problems/koko-eating-bananas/) | 🟠 |
| \- | [剑指 Offer II 073. 狒狒吃香蕉](https://leetcode.cn/problems/nZZqjQ/) | 🟠 |

**———–**

我们前文 [我写了首诗，把二分搜索变成了默写题](https://labuladong.gitee.io/algo/2/20/29/) 详细介绍了二分搜索的细节问题，探讨了「搜索一个元素」，「搜索左侧边界」，「搜索右侧边界」这三个情况，教你如何写出正确无 bug 的二分搜索算法。

**但是前文总结的二分搜索代码框架仅仅局限于「在有序数组中搜索指定元素」这个基本场景，具体的算法问题没有这么直接，可能你都很难看出这个问题能够用到二分搜索**。

所以本文就来总结一套二分搜索算法运用的框架套路，帮你在遇到二分搜索算法相关的实际问题时，能够有条理地思考分析，步步为营，写出答案。

### 原始的二分搜索代码

二分搜索的原型就是在「**有序数组**」中搜索一个元素 `target`，返回该元素对应的索引。

如果该元素不存在，那可以返回一个什么特殊值，这种细节问题只要微调算法实现就可实现。

还有一个重要的问题，如果「**有序数组**」中存在多个 `target` 元素，那么这些元素肯定挨在一起，这里就涉及到算法应该返回最左侧的那个 `target` 元素的索引还是最右侧的那个 `target` 元素的索引，也就是所谓的「搜索左侧边界」和「搜索右侧边界」，这个也可以通过微调算法的代码来实现。

**我们前文 [我写了首诗，把二分搜索变成了默写题](https://labuladong.gitee.io/algo/2/20/29/) 详细探讨了上述问题，对这块还不清楚的读者建议复习前文**，已经搞清楚基本二分搜索算法的读者可以继续看下去。

**在具体的算法问题中，常用到的是「搜索左侧边界」和「搜索右侧边界」这两种场景**，很少有让你单独「搜索一个元素」。

因为算法题一般都让你求最值，比如让你求吃香蕉的「最小速度」，让你求轮船的「最低运载能力」，求最值的过程，必然是搜索一个边界的过程，所以后面我们就详细分析一下这两种搜索边界的二分算法代码。

「搜索左侧边界」的二分搜索算法的具体代码实现如下：

```
// 搜索左侧边界
int left_bound(int[] nums, int target) {
    if (nums.length == 0) return -1;
    int left = 0, right = nums.length;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            // 当找到 target 时，收缩右侧边界
            right = mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid;
        }
    }
    return left;
}
```

假设输入的数组 `nums = [1,2,3,3,3,5,7]`，想搜索的元素 `target = 3`，那么算法就会返回索引 2。

如果画一个图，就是这样：

[![](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%88%86%e8%bf%90%e7%94%a8/1.jpeg)](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%88%86%e8%bf%90%e7%94%a8/1.jpeg)

「搜索右侧边界」的二分搜索算法的具体代码实现如下：

```
// 搜索右侧边界
int right_bound(int[] nums, int target) {
    if (nums.length == 0) return -1;
    int left = 0, right = nums.length;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            // 当找到 target 时，收缩左侧边界
            left = mid + 1;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid;
        }
    }
    return left - 1;
}
```

输入同上，那么算法就会返回索引 4，如果画一个图，就是这样：

[![](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%88%86%e8%bf%90%e7%94%a8/2.jpeg)](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%88%86%e8%bf%90%e7%94%a8/2.jpeg)

好，上述内容都属于复习，我想读到这里的读者应该都能理解。记住上述的图像，所有能够抽象出上述图像的问题，都可以使用二分搜索解决。

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

应合作方要求，本文不便在此发布，请扫码关注回复关键词「二分」或 [点这里](https://appktavsiei5995.pc.xiaoe-tech.com/detail/i_627cce2de4b01a4851fe0ed1/1) 查看：

[![](https://labuladong.gitee.io/algo/images/qrcode.jpg)](https://labuladong.gitee.io/algo/images/qrcode.jpg)

**共同维护高质量学习环境，评论礼仪[见这里](https://mp.weixin.qq.com/s/YdSoYZS0QjZpbphQlpHyyA)，违者直接拉黑不解释**