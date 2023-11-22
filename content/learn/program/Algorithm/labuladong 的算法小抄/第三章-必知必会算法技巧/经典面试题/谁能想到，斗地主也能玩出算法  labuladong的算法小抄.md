---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:28:59
tags: [learn/program/algorithm/labuladong/技巧/面试]
title: 谁能想到，斗地主也能玩出算法  labuladong的算法小抄
---
# 谁能想到，斗地主也能玩出算法 Labuladong的算法小抄
## 谁能想到，斗地主也能玩出算法

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

读完本文，你不仅学会了算法套路，还可以顺便解决如下题目：

| LeetCode | 力扣 | 难度 |
| --- | --- | --- |
| [659\. Split Array into Consecutive Subsequences](https://leetcode.com/problems/split-array-into-consecutive-subsequences/) | [659\. 分割数组为连续子序列](https://leetcode.cn/problems/split-array-into-consecutive-subsequences/) | 🟠 |

**———–**

斗地主中，大小连续的牌可以作为顺子，有时候我们把对子拆掉，结合单牌，可以组合出更多的顺子，可能更容易赢。

那么如何合理拆分手上的牌，合理地拆出顺子呢？我们今天看一道非常有意思的算法题，连续子序列的划分问题。

这是力扣第 659 题「 [分割数组为连续子序列](https://leetcode.cn/problems/split-array-into-consecutive-subsequences/)」，题目很简单：

给你输入一个**升序排列**的数组 `nums`（可能包含重复数字），请你判断 `nums` 是否能够被分割成若干个**长度至少为 3** 的子序列，每个子序列都由连续的整数组成。

函数签名如下：

```
bool isPossible(vector<int>& nums);
```

比如题目举的例子，输入 `nums = [1,2,3,3,4,4,5,5]`，算法返回 true。

因为 `nums` 可以被分割成 `[1,2,3,4,5]` 和 `[3,4,5]` 两个包含连续整数子序列。

但如果输入 `nums = [1,2,3,4,4,5]`，算法返回 false，因为无法分割成两个长度至少为 3 的连续子序列。

**对于这种涉及连续整数的问题，应该条件反射地想到排序**，不过题目说了，输入的 `nums` 本就是排好序的。

那么，我们如何判断 `nums` 是否能够被划分成若干符合条件的子序列呢？

类似前文 [回溯算法进行集合划分](https://labuladong.gitee.io/algo/4/31/105/)，我们想把 `nums` 的元素划分到若干个子序列中，其实就是下面这个代码逻辑：

```
for (int v : nums) {
    if (...) {
        // 将 v 分配到某个子序列中
    } else {
        // 实在无法分配 v
        return false;
    }
    return true;
}
```

关键在于，我们怎么知道当前元素 `v` 如何进行分配呢？

肯定得分情况讨论，把情况讨论清楚了，题目也就做出来了。

总共有两种情况：

**1、当前元素 `v` 自成一派，「以自己开头」构成一个长度至少为 3 的序列**。

比如输入 `nums = [1,2,3,6,7,8]`，遍历到元素 `6` 时，它只能自己开头形成一个符合条件的子序列 `[6,7,8]`。

**2、当前元素 `v` 接到已经存在的子序列后面**。

比如输入 `nums = [1,2,3,4,5]`，遍历到元素 `4` 时，它只能接到已经存在的子序列 `[1,2,3]` 后面。它没办法自成开头形成新的子序列，因为少了个 `6`。

但是，如果这两种情况都可以，应该如何选择？

比如说，输入 `nums = [1,2,3,4,5,5,6,7]`，对于元素 `4`，你说它应该形成一个新的子序列 `[4,5,6]` 还是接到子序列 `[1,2,3]` 后面呢？

显然，`nums` 数组的正确划分方法是分成 `[1,2,3,4,5]` 和 `[5,6,7]`，所以元素 `4` 应该**优先判断自己是否能够接到其他序列后面**，如果不行，再判断是否可以作为新的子序列开头。

这就是整体的思路，想让算法代码实现这两个选择，需要两个哈希表来做辅助：

`freq` 哈希表帮助一个元素判断自己是否能够作为开头，`need` 哈希表帮助一个元素判断自己是否可以被接到其他序列后面。

**`freq` 记录每个元素出现的次数**，比如 `freq[3] == 2` 说明元素 `3` 在 `nums` 中出现了 2 次。

那么如果我发现 `freq[3], freq[4], freq[5]` 都是大于 0 的，那就说明元素 `3` 可以作为开头组成一个长度为 3 的子序列。

**`need` 记录哪些元素可以被接到其他子序列后面**。

比如说现在已经组成了两个子序列 `[1,2,3,4]` 和 `[2,3,4]`，那么 `need[5]` 的值就应该是 2，说明对元素 `5` 的需求为 2。

明白了这两个哈希表的作用，我们就可以看懂解法了：

```
bool isPossible(vector<int>& nums) {

    unordered_map<int, int> freq, need;

    // 统计 nums 中元素的频率
    for (int v : nums) freq[v]++;
    
    for (int v : nums) {
        if (freq[v] == 0) {
            // 已经被用到其他子序列中
            continue;
        }
        // 先判断 v 是否能接到其他子序列后面
        if (need.count(v) && need[v] > 0) {
            // v 可以接到之前的某个序列后面
            freq[v]--;
            // 对 v 的需求减一
            need[v]--;
            // 对 v + 1 的需求加一
            need[v + 1]++; 
        } else if (freq[v] > 0 && freq[v + 1] > 0 && freq[v + 2] > 0) {
            // 将 v 作为开头，新建一个长度为 3 的子序列 [v,v+1,v+2]
            freq[v]--;
            freq[v + 1]--;
            freq[v + 2]--;
            // 对 v + 3 的需求加一
            need[v + 3]++;
        } else {
            // 两种情况都不符合，则无法分配
            return false;
        }
    }

    return true;
}
```

至此，这道题就解决了。

那你可能会说，斗地主里面顺子至少要 5 张连续的牌，我们这道题只计算长度最小为 3 的子序列，怎么办？

很简单，把我们的 else if 分支修改一下，连续判断 `v` 之后的连续 5 个元素就行了。

那么，我们再难为难为自己，如果我想要的不只是一个布尔值，我想要你给我把子序列都打印出来，怎么办？

其实这也很好实现，只要修改 `need`，不仅记录对某个元素的需求个数，而且记录具体是哪些子序列产生的需求：

```
// need[6] = 2 说明有两个子序列需要 6
unordered_map<int, int> need;

// need[6] = {
//     {3,4,5},
//     {2,3,4,5},
// }
// 记录哪两个子序列需要 6
unordered_map<int, vector<vector<int>>> need;
```

这样，我们稍微修改一下之前的代码就行了：

```
bool isPossible(vector<int>& nums) {
    unordered_map<int, int> freq;
    unordered_map<int, vector<vector<int>>> need;
    
    for (int v : nums) freq[v]++;
    
    for (int v : nums) {
        if (freq[v] == 0) {
            continue;
        }

        if (need.count(v) && need[v].size() > 0) {
            // v 可以接到之前的某个序列后面
            freq[v]--;
            // 随便取一个需要 v 的子序列
            vector<int> seq = need[v].back();
            need[v].pop_back();
            // 把 v 接到这个子序列后面
            seq.push_back(v);
            // 这个子序列的需求变成了 v + 1
            need[v + 1].push_back(seq);

        } else if (freq[v] > 0 && freq[v + 1] > 0 && freq[v + 2] > 0) {
            // 可以将 v 作为开头
            freq[v]--;
            freq[v + 1]--;
            freq[v + 2]--;
            // 新建一个长度为 3 的子序列 [v,v + 1,v + 2]
            vector<int> seq{v, v + 1, v + 2};
            // 对 v + 3 的需求加一
            need[v + 3].push_back(seq);

        } else {
            return false;
        }
    }
    
    // 打印切分出的所有子序列
    for (auto it : need) {
        for (vector<int>& seq : it.second) {
            for (int v : seq) {
                cout << v << " ";
            }
            cout << endl;
        }
    }
    
    return true;
}
```

这样，我们记录具体子序列的需求也实现了。

如果本文对你有帮助，点个赞，微信会给你推荐更多相似文章。

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

**《labuladong 的算法小抄》已经出版，关注公众号查看详情；后台回复关键词「**进群**」可加入算法群；回复「**全家桶**」可下载配套 PDF 和刷题全家桶**：

[![](https://labuladong.gitee.io/algo/images/souyisou2.png)](https://labuladong.gitee.io/algo/images/souyisou2.png)