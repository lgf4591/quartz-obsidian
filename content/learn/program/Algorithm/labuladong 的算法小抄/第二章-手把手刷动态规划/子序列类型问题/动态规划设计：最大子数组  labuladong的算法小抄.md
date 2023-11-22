---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:29:43
tags: [learn/program/algorithm/labuladong/动态规划/子序列问题]
title: 动态规划设计：最大子数组  labuladong的算法小抄
---
# 动态规划设计：最大子数组 Labuladong的算法小抄
## 动态规划设计：最大子数组

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

读完本文，你不仅学会了算法套路，还可以顺便解决如下题目：

| LeetCode | 力扣 | 难度 |
| --- | --- | --- |
| [53\. Maximum Subarray](https://leetcode.com/problems/maximum-subarray/) | [53\. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/) | 🟢 |
| \- | [剑指 Offer 42. 连续子数组的最大和](https://leetcode.cn/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/) | 🟢 |

**———–**

力扣第 53 题「 [最大子序和](https://leetcode.cn/problems/maximum-subarray/)」问题和前文讲过的 [经典动态规划：最长递增子序列](https://labuladong.gitee.io/algo/3/26/76/) 的套路非常相似，代表着一类比较特殊的动态规划问题的思路，题目如下：

给你输入一个整数数组 `nums`，请你找在其中找一个和最大的子数组，返回这个子数组的和。函数签名如下：

```
int maxSubArray(int[] nums);
```

比如说输入 `nums = [-3,1,3,-1,2,-4,2]`，算法返回 5，因为最大子数组 `[1,3,-1,2]` 的和为 5。

其实第一次看到这道题，我首先想到的是滑动窗口算法，因为我们前文说过嘛，滑动窗口算法就是专门处理子串/子数组问题的，这里不就是子数组问题么？

前文 [滑动窗口算法框架详解](https://labuladong.gitee.io/algo/2/20/27/) 中讲过，想用滑动窗口算法，先问自己几个问题：

1、什么时候应该扩大窗口？

2、什么时候应该缩小窗口？

3、什么时候更新答案？

我之前认为这题用不了滑动窗口算法，因为我认为 `nums` 中包含负数，所以无法确定什么时候扩大和缩小窗口。但经读者评论的启发，发现这道题确实是可以用滑动窗口技巧解决的。

**我们可以在窗口内元素之和大于等于 0 时扩大窗口，在窗口内元素之和小于 0 时缩小窗口，在每次移动窗口时更新答案**。先直接看解法代码，待会儿再解释：

```
int maxSubArray(int[] nums) {
    int left = 0, right = 0;
    int windowSum = 0, maxSum = Integer.MIN_VALUE;
    while(right < nums.length){
        // 扩大窗口并更新窗口内的元素和
        windowSum += nums[right];
        right++;

        // 更新答案
        maxSum = windowSum > maxSum ? windowSum : maxSum;

        // 判断窗口是否要收缩
        while(windowSum < 0) {
            // 缩小窗口并更新窗口内的元素和
            windowSum -=  nums[left];
            left++;
        }
    }
    return maxSum;
}
```

结合前文 [滑动窗口算法框架详解](https://labuladong.gitee.io/algo/2/20/27/) 给出的滑动窗口代码框架，这段代码的结构应该很清晰，我主要解释一下为什么这个逻辑是正确的。

首先讨论一种特殊情况，就是 `nums` 中全是负数的时候，此时算法是可以得到正确答案的。

接下来讨论一般情况，`nums` 中有正有负，这种情况下元素和最大的那个子数组一定是以正数开头的（以负数开头的话，把这个负数去掉，就可以得到和更大的子数组了，与假设相矛盾）。那么此时我们需要穷举所有以正数开头的子数组，计算他们的元素和，找到元素和最大的那个子数组。

说到这里，解法代码的逻辑应该就清晰了。算法只有在窗口元素和大于 0 时才会不断扩大窗口，并且在扩大窗口时更新答案，这其实就是在穷举所有正数开头的子数组，寻找子数组和最大的那个，所以这段代码能够得到正确的结果。

### 动态规划思路

解决这个问题还可以用动态规划技巧解决，但是 `dp` 数组的定义比较特殊。按照我们常规的动态规划思路，一般是这样定义 `dp` 数组：

**`nums[0..i]` 中的「最大的子数组和」为 `dp[i]`**。

如果这样定义的话，整个 `nums` 数组的「最大子数组和」就是 `dp[n-1]`。如何找状态转移方程呢？按照数学归纳法，假设我们知道了 `dp[i-1]`，如何推导出 `dp[i]` 呢？

如下图，按照我们刚才对 `dp` 数组的定义，`dp[i] = 5` ，也就是等于 `nums[0..i]` 中的最大子数组和：

[![](https://labuladong.gitee.io/algo/images/%e6%9c%80%e5%a4%a7%e5%ad%90%e6%95%b0%e7%bb%84/1.jpeg)](https://labuladong.gitee.io/algo/images/%e6%9c%80%e5%a4%a7%e5%ad%90%e6%95%b0%e7%bb%84/1.jpeg)

那么在上图这种情况中，利用数学归纳法，你能用 `dp[i]` 推出 `dp[i+1]` 吗？

**实际上是不行的，因为子数组一定是连续的，按照我们当前 `dp` 数组定义，并不能保证 `nums[0..i]` 中的最大子数组与 `nums[i+1]` 是相邻的**，也就没办法从 `dp[i]` 推导出 `dp[i+1]`。

所以说我们这样定义 `dp` 数组是不正确的，无法得到合适的状态转移方程。对于这类子数组问题，我们就要重新定义 `dp` 数组的含义：

**以 `nums[i]` 为结尾的「最大子数组和」为 `dp[i]`**。

这种定义之下，想得到整个 `nums` 数组的「最大子数组和」，不能直接返回 `dp[n-1]`，而需要遍历整个 `dp` 数组：

```
int res = Integer.MIN_VALUE;
for (int i = 0; i < n; i++) {
    res = Math.max(res, dp[i]);
}
return res;
```

依然使用数学归纳法来找状态转移关系：假设我们已经算出了 `dp[i-1]`，如何推导出 `dp[i]` 呢？

可以做到，`dp[i]` 有两种「选择」，要么与前面的相邻子数组连接，形成一个和更大的子数组；要么不与前面的子数组连接，自成一派，自己作为一个子数组。

如何选择？既然要求「最大子数组和」，当然选择结果更大的那个啦：

```
// 要么自成一派，要么和前面的子数组合并
dp[i] = Math.max(nums[i], nums[i] + dp[i - 1]);
```

综上，我们已经写出了状态转移方程，就可以直接写出解法了：

```
int maxSubArray(int[] nums) {
    int n = nums.length;
    if (n == 0) return 0;
    // 定义：dp[i] 记录以 nums[i] 为结尾的「最大子数组和」
    int[] dp = new int[n];
    // base case
    // 第一个元素前面没有子数组
    dp[0] = nums[0];
    // 状态转移方程
    for (int i = 1; i < n; i++) {
        dp[i] = Math.max(nums[i], nums[i] + dp[i - 1]);
    }
    // 得到 nums 的最大子数组
    int res = Integer.MIN_VALUE;
    for (int i = 0; i < n; i++) {
        res = Math.max(res, dp[i]);
    }
    return res;
}
```

以上解法时间复杂度是 O(N)，空间复杂度也是 O(N)，较暴力解法已经很优秀了，不过**注意到 `dp[i]` 仅仅和 `dp[i-1]` 的状态有关**，那么我们可以施展前文 [动态规划的降维打击：空间压缩技巧](https://labuladong.gitee.io/algo/3/25/73/) 讲的技巧进行进一步优化，将空间复杂度降低：

```
int maxSubArray(int[] nums) {
    int n = nums.length;
    if (n == 0) return 0;
    // base case
    int dp_0 = nums[0];
    int dp_1 = 0, res = dp_0;

    for (int i = 1; i < n; i++) {
        // dp[i] = max(nums[i], nums[i] + dp[i-1])
        dp_1 = Math.max(nums[i], nums[i] + dp_0);
        dp_0 = dp_1;
        // 顺便计算最大的结果
        res = Math.max(res, dp_1);
    }
    
    return res;
}
```

### 前缀和思路

在动态规划解法中，我们通过状态转移方程推导以 `nums[i]` 结尾的最大子数组和，其实用前文 [小而美的算法技巧：前缀和数组](https://labuladong.gitee.io/algo/2/20/24/) 讲过的前缀和数组也可以达到相同的效果。

回顾一下，前缀和数组 `preSum` 就是 `nums` 元素的累加和，`preSum[i+1] - preSum[j]` 其实就是子数组 `nums[j..i]` 之和（根据 `preSum` 数组的实现，索引 0 是占位符，所以 `i` 有一位索引偏移）。

**那么反过来想，以 `nums[i]` 为结尾的最大子数组之和是多少？其实就是 `preSum[i+1] - min(preSum[0..i])`**。

所以，我们可以利用前缀和数组计算以每个元素结尾的子数组之和，进而得到和最大的子数组：

```
// 前缀和技巧解题
int maxSubArray(int[] nums) {
    int n = nums.length;
    int[] preSum = new int[n + 1];
    preSum[0] = 0;
    // 构造 nums 的前缀和数组
    for (int i = 1; i <= n; i++) {
        preSum[i] = preSum[i - 1] + nums[i - 1];
    }
    
    int res = Integer.MIN_VALUE;
    int minVal = Integer.MAX_VALUE;
    for (int i = 0; i < n; i++) {
        // 维护 minVal 是 preSum[0..i] 的最小值
        minVal = Math.min(minVal, preSum[i]);
        // 以 nums[i] 结尾的最大子数组和就是 preSum[i+1] - min(preSum[0..i])
        res = Math.max(res, preSum[i + 1] - minVal);
    }
    return res;
}
```

至此，前缀和解法也完成了。

简单总结下动态规划解法吧，虽然说状态转移方程确实有点玄学，但大部分还是有些规律可循的，跑不出那几个套路。像子数组、子序列这类问题，你就可以尝试定义 `dp[i]` 是以 `nums[i]` 为结尾的最大子数组和/最长递增子序列，因为这样定义更容易将 `dp[i+1]` 和 `dp[i]` 建立起联系，利用数学归纳法写出状态转移方程。

___

**引用本文的题目**

**安装 [我的 Chrome 刷题插件](https://mp.weixin.qq.com/s/X-fE9sR4BLi6T9pn7xP4pg) 点开下列题目可直接查看解题思路：**

| LeetCode | 力扣 |
| --- | --- |
| [256\. Paint House](https://leetcode.com/problems/paint-house/?show=1)🔒 | [256\. 粉刷房子](https://leetcode.cn/problems/paint-house/?show=1)🔒 |
| \- | [剑指 Offer 42. 连续子数组的最大和](https://leetcode.cn/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/?show=1) |
| \- | [剑指 Offer II 091. 粉刷房子](https://leetcode.cn/problems/JEj789/?show=1) |

___

**引用本文的文章**

-   [动态规划之子序列问题解题模板](https://labuladong.gitee.io/algo/3/26/79/)
-   [动态规划设计：最长递增子序列](https://labuladong.gitee.io/algo/3/26/76/)
-   [我的刷题心得](https://labuladong.gitee.io/algo/1/3/)

___

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

**《labuladong 的算法小抄》已经出版，关注公众号查看详情；后台回复关键词「**进群**」可加入算法群；回复「**全家桶**」可下载配套 PDF 和刷题全家桶**：

[![](https://labuladong.gitee.io/algo/images/souyisou2.png)](https://labuladong.gitee.io/algo/images/souyisou2.png)