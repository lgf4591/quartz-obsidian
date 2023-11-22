---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:34:12
tags: [learn/program/algorithm/labuladong/核心框架]
title: 我写了首诗，把二分搜索算法变成了默写题  labuladong的算法小抄
---
# 我写了首诗，把二分搜索算法变成了默写题 Labuladong的算法小抄
## 我写了首诗，把二分搜索算法变成了默写题

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

读完本文，你不仅学会了算法套路，还可以顺便解决如下题目：

| LeetCode | 力扣 | 难度 |
| --- | --- | --- |
| [34\. Find First and Last Position of Element in Sorted Array](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) | [34\. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/) | 🟠 |
| [704\. Binary Search](https://leetcode.com/problems/binary-search/) | [704\. 二分查找](https://leetcode.cn/problems/binary-search/) | 🟢 |
| \- | [剑指 Offer 53 - I. 在排序数组中查找数字 I](https://leetcode.cn/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/) | 🟢 |

**———–**

> 本文有视频版： [二分搜索核心框架套路](https://www.bilibili.com/video/BV1Gt4y1b79Q/)

本文是前文 [二分搜索详解](https://mp.weixin.qq.com/s/uA2suoVykENmCQcKFMOSuQ) 的修订版，添加了对二分搜索算法更详细的分析。

先给大家讲个笑话乐呵一下：

有一天阿东到图书馆借了 N 本书，出图书馆的时候，警报响了，于是保安把阿东拦下，要检查一下哪本书没有登记出借。阿东正准备把每一本书在报警器下过一下，以找出引发警报的书，但是保安露出不屑的眼神：你连二分查找都不会吗？于是保安把书分成两堆，让第一堆过一下报警器，报警器响；于是再把这堆书分成两堆…… 最终，检测了 logN 次之后，保安成功的找到了那本引起警报的书，露出了得意和嘲讽的笑容。于是阿东背着剩下的书走了。

从此，图书馆丢了 N - 1 本书（手动狗头）。

二分查找并不简单，Knuth 大佬（发明 KMP 算法的那位）都说二分查找：**思路很简单，细节是魔鬼**。很多人喜欢拿整型溢出的 bug 说事儿，但是二分查找真正的坑根本就不是那个细节问题，而是在于到底要给 `mid` 加一还是减一，while 里到底用 `<=` 还是 `<`。

你要是没有正确理解这些细节，写二分肯定就是玄学编程，有没有 bug 只能靠菩萨保佑（谁写谁知道）。我特意写了一首诗来歌颂该算法，概括本文的主要内容，建议保存（手动狗头）：

[![](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%88%86%e6%9f%a5%e6%89%be/poem.png)](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%88%86%e6%9f%a5%e6%89%be/poem.png)

本文就来探究几个最常用的二分查找场景：寻找一个数、寻找左侧边界、寻找右侧边界。而且，我们就是要深入细节，比如不等号是否应该带等号，`mid` 是否应该加一等等。分析这些细节的差异以及出现这些差异的原因，保证你能灵活准确地写出正确的二分查找算法。

另外再声明一下，对于二分搜索的每一个场景，本文还会探讨多种代码写法，目的是为了让你理解出现这些细微差异的本质原因，最起码你看到别人的代码时不会懵逼。实际上这些写法没有优劣之分，你喜欢哪种就用哪种好了。

### 零、二分查找框架

```
int binarySearch(int[] nums, int target) {
    int left = 0, right = ...;

    while(...) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            ...
        } else if (nums[mid] < target) {
            left = ...
        } else if (nums[mid] > target) {
            right = ...
        }
    }
    return ...;
}
```

**分析二分查找的一个技巧是：不要出现 else，而是把所有情况用 else if 写清楚，这样可以清楚地展现所有细节**。本文都会使用 else if，旨在讲清楚，读者理解后可自行简化。

其中 `...` 标记的部分，就是可能出现细节问题的地方，当你见到一个二分查找的代码时，首先注意这几个地方。后文用实例分析这些地方能有什么样的变化。

**另外提前说明一下，计算 `mid` 时需要防止溢出**，代码中 `left + (right - left) / 2` 就和 `(left + right) / 2` 的结果相同，但是有效防止了 `left` 和 `right` 太大，直接相加导致溢出的情况。

### 一、寻找一个数（基本的二分搜索）

这个场景是最简单的，可能也是大家最熟悉的，即搜索一个数，如果存在，返回其索引，否则返回 -1。

```
int binarySearch(int[] nums, int target) {
    int left = 0; 
    int right = nums.length - 1; // 注意

    while(left <= right) {
        int mid = left + (right - left) / 2;
        if(nums[mid] == target)
            return mid; 
        else if (nums[mid] < target)
            left = mid + 1; // 注意
        else if (nums[mid] > target)
            right = mid - 1; // 注意
    }
    return -1;
}
```

这段代码可以解决力扣第 704 题「 [二分查找](https://leetcode.cn/problems/binary-search/)」，但我们深入探讨一下其中的细节。

**1、为什么 while 循环的条件中是 <=，而不是 <**？

答：因为初始化 `right` 的赋值是 `nums.length - 1`，即最后一个元素的索引，而不是 `nums.length`。

这二者可能出现在不同功能的二分查找中，区别是：前者相当于两端都闭区间 `[left, right]`，后者相当于左闭右开区间 `[left, right)`，因为索引大小为 `nums.length` 是越界的。

我们这个算法中使用的是前者 `[left, right]` 两端都闭的区间。**这个区间其实就是每次进行搜索的区间**。

什么时候应该停止搜索呢？当然，找到了目标值的时候可以终止：

```
    if(nums[mid] == target)
        return mid; 
```

但如果没找到，就需要 while 循环终止，然后返回 -1。那 while 循环什么时候应该终止？**搜索区间为空的时候应该终止**，意味着你没得找了，就等于没找到嘛。

`while(left <= right)` 的终止条件是 `left == right + 1`，写成区间的形式就是 `[right + 1, right]`，或者带个具体的数字进去 `[3, 2]`，可见**这时候区间为空**，因为没有数字既大于等于 3 又小于等于 2 的吧。所以这时候 while 循环终止是正确的，直接返回 -1 即可。

`while(left < right)` 的终止条件是 `left == right`，写成区间的形式就是 `[right, right]`，或者带个具体的数字进去 `[2, 2]`，**这时候区间非空**，还有一个数 2，但此时 while 循环终止了。也就是说这区间 `[2, 2]` 被漏掉了，索引 2 没有被搜索，如果这时候直接返回 -1 就是错误的。

当然，如果你非要用 `while(left < right)` 也可以，我们已经知道了出错的原因，就打个补丁好了：

```
    //...
    while(left < right) {
        // ...
    }
    return nums[left] == target ? left : -1;
```

**2、为什么 `left = mid + 1`，`right = mid - 1`？我看有的代码是 `right = mid` 或者 `left = mid`，没有这些加加减减，到底怎么回事，怎么判断**？

答：这也是二分查找的一个难点，不过只要你能理解前面的内容，就能够很容易判断。

刚才明确了「搜索区间」这个概念，而且本算法的搜索区间是两端都闭的，即 `[left, right]`。那么当我们发现索引 `mid` 不是要找的 `target` 时，下一步应该去搜索哪里呢？

当然是去搜索区间 `[left, mid-1]` 或者区间 `[mid+1, right]` 对不对？**因为 `mid` 已经搜索过，应该从搜索区间中去除**。

**3、此算法有什么缺陷**？

答：至此，你应该已经掌握了该算法的所有细节，以及这样处理的原因。但是，这个算法存在局限性。

比如说给你有序数组 `nums = [1,2,2,2,3]`，`target` 为 2，此算法返回的索引是 2，没错。但是如果我想得到 `target` 的左侧边界，即索引 1，或者我想得到 `target` 的右侧边界，即索引 3，这样的话此算法是无法处理的。

这样的需求很常见，**你也许会说，找到一个 `target`，然后向左或向右线性搜索不行吗？可以，但是不好，因为这样难以保证二分查找对数级的复杂度了**。

我们后续的算法就来讨论这两种二分查找的算法。

### 二、寻找左侧边界的二分搜索

以下是最常见的代码形式，其中的标记是需要注意的细节：

```
int left_bound(int[] nums, int target) {
    int left = 0;
    int right = nums.length; // 注意
    
    while (left < right) { // 注意
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            right = mid;
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid; // 注意
        }
    }
    return left;
}
```

**1、为什么 while 中是 `<` 而不是 `<=`**?

答：用相同的方法分析，因为 `right = nums.length` 而不是 `nums.length - 1`。因此每次循环的「搜索区间」是 `[left, right)` 左闭右开。

`while(left < right)` 终止的条件是 `left == right`，此时搜索区间 `[left, left)` 为空，所以可以正确终止。

> PS：这里先要说一个搜索左右边界和上面这个算法的一个区别，也是很多读者问的：**刚才的 `right` 不是 `nums.length - 1` 吗，为啥这里非要写成 `nums.length` 使得「搜索区间」变成左闭右开呢**？
>
> 因为对于搜索左右侧边界的二分查找，这种写法比较普遍，我就拿这种写法举例了，保证你以后遇到这类代码可以理解。你非要用两端都闭的写法反而更简单，我会在后面写相关的代码，把三种二分搜索都用一种两端都闭的写法统一起来，你耐心往后看就行了。

**2、为什么没有返回 -1 的操作？如果 `nums` 中不存在 `target` 这个值，怎么办**？

答：其实很简单，在返回的时候额外判断一下 `nums[left]` 是否等于 `target` 就行了，如果不等于，就说明 `target` 不存在。

不过我们得考察一下 `left` 的取值范围，免得索引越界。假如输入的 `target` 非常大，那么就会一直触发 `nums[mid] < target` 的 if 条件，`left` 会一直向右侧移动，直到等于 `right`，while 循环结束。

由于这里 `right` 初始化为 `nums.length`，所以 `left` 变量的取值区间是闭区间 `[0, nums.length]`，那么我们在检查 `nums[left]` 之前需要额外判断一下，防止索引越界：

```
while (left < right) {
    //...
}
// 此时 target 比所有数都大，返回 -1
if (left == nums.length) return -1;
// 判断一下 nums[left] 是不是 target
return nums[left] == target ? left : -1;
```

**3、为什么 `left = mid + 1`，`right = mid` ？和之前的算法不一样**？

答：这个很好解释，因为我们的「搜索区间」是 `[left, right)` 左闭右开，所以当 `nums[mid]` 被检测之后，下一步应该去 `mid` 的左侧或者右侧区间搜索，即 `[left, mid)` 或 `[mid + 1, right)`。

**4、为什么该算法能够搜索左侧边界**？

答：关键在于对于 `nums[mid] == target` 这种情况的处理：

```
    if (nums[mid] == target)
        right = mid;
```

可见，找到 target 时不要立即返回，而是缩小「搜索区间」的上界 `right`，在区间 `[left, mid)` 中继续搜索，即不断向左收缩，达到锁定左侧边界的目的。

**5、为什么返回 `left` 而不是 `right`**？

答：都是一样的，因为 while 终止的条件是 `left == right`。

**6、能不能想办法把 `right` 变成 `nums.length - 1`，也就是继续使用两边都闭的「搜索区间」？这样就可以和第一种二分搜索在某种程度上统一起来了**。

答：当然可以，只要你明白了「搜索区间」这个概念，就能有效避免漏掉元素，随便你怎么改都行。下面我们严格根据逻辑来修改：

因为你非要让搜索区间两端都闭，所以 `right` 应该初始化为 `nums.length - 1`，while 的终止条件应该是 `left == right + 1`，也就是其中应该用 `<=`：

```
int left_bound(int[] nums, int target) {
    // 搜索区间为 [left, right]
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        // if else ...
    }
```

因为搜索区间是两端都闭的，且现在是搜索左侧边界，所以 `left` 和 `right` 的更新逻辑如下：

```
if (nums[mid] < target) {
    // 搜索区间变为 [mid+1, right]
    left = mid + 1;
} else if (nums[mid] > target) {
    // 搜索区间变为 [left, mid-1]
    right = mid - 1;
} else if (nums[mid] == target) {
    // 收缩右侧边界
    right = mid - 1;
}
```

和刚才相同，如果想在找不到 `target` 的时候返回 -1，那么检查一下 `nums[left]` 和 `target` 是否相等即可：

```
// 此时 target 比所有数都大，返回 -1
if (left == nums.length) return -1;
// 判断一下 nums[left] 是不是 target
return nums[left] == target ? left : -1;
```

至此，整个算法就写完了，完整代码如下：

```
int left_bound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    // 搜索区间为 [left, right]
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            // 搜索区间变为 [mid+1, right]
            left = mid + 1;
        } else if (nums[mid] > target) {
            // 搜索区间变为 [left, mid-1]
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 收缩右侧边界
            right = mid - 1;
        }
    }
    // 判断 target 是否存在于 nums 中
    // 此时 target 比所有数都大，返回 -1
    if (left == nums.length) return -1;
    // 判断一下 nums[left] 是不是 target
    return nums[left] == target ? left : -1;
}
```

这样就和第一种二分搜索算法统一了，都是两端都闭的「搜索区间」，而且最后返回的也是 `left` 变量的值。只要把住二分搜索的逻辑，两种形式大家看自己喜欢哪种记哪种吧。

### 三、寻找右侧边界的二分查找

类似寻找左侧边界的算法，这里也会提供两种写法，还是先写常见的左闭右开的写法，只有两处和搜索左侧边界不同：

```
int right_bound(int[] nums, int target) {
    int left = 0, right = nums.length;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            left = mid + 1; // 注意
        } else if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid;
        }
    }
    return left - 1; // 注意
}
```

**1、为什么这个算法能够找到右侧边界**？

答：类似地，关键点还是这里：

```
if (nums[mid] == target) {
    left = mid + 1;
```

当 `nums[mid] == target` 时，不要立即返回，而是增大「搜索区间」的左边界 `left`，使得区间不断向右靠拢，达到锁定右侧边界的目的。

**2、为什么最后返回 `left - 1` 而不像左侧边界的函数，返回 `left`？而且我觉得这里既然是搜索右侧边界，应该返回 `right` 才对**。

答：首先，while 循环的终止条件是 `left == right`，所以 `left` 和 `right` 是一样的，你非要体现右侧的特点，返回 `right - 1` 好了。

至于为什么要减一，这是搜索右侧边界的一个特殊点，关键在锁定右边界时的这个条件判断：

```
// 增大 left，锁定右侧边界
if (nums[mid] == target) {
    left = mid + 1;
    // 这样想: mid = left - 1
```

[![](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%88%86%e6%9f%a5%e6%89%be/3.jpg)](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%88%86%e6%9f%a5%e6%89%be/3.jpg)

因为我们对 `left` 的更新必须是 `left = mid + 1`，就是说 while 循环结束时，`nums[left]` 一定不等于 `target` 了，而 `nums[left-1]` 可能是 `target`。

至于为什么 `left` 的更新必须是 `left = mid + 1`，当然是为了把 `nums[mid]` 排除出搜索区间，这里就不再赘述。

**3、为什么没有返回 -1 的操作？如果 `nums` 中不存在 `target` 这个值，怎么办**？

答：只要在最后判断一下 `nums[left-1]` 是不是 `target` 就行了。

类似之前的左侧边界搜索，`left` 的取值范围是 `[0, nums.length]`，但由于我们最后返回的是 `left - 1`，所以 `left` 取值为 0 的时候会造成索引越界，额外处理一下即可正确地返回 -1：

```
while (left < right) {
    // ...
}
// 判断 target 是否存在于 nums 中
// 此时 left - 1 索引越界
if (left - 1 < 0) return -1;
// 判断一下 nums[left] 是不是 target
return nums[left - 1] == target ? (left - 1) : -1;
```

**4、是否也可以把这个算法的「搜索区间」也统一成两端都闭的形式呢？这样这三个写法就完全统一了，以后就可以闭着眼睛写出来了**。

答：当然可以，类似搜索左侧边界的统一写法，其实只要改两个地方就行了：

```
int right_bound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 这里改成收缩左侧边界即可
            left = mid + 1;
        }
    }
    // 最后改成返回 left - 1
    if (left - 1 < 0) return -1;
    return nums[left - 1] == target ? (left - 1) : -1;
}
```

当然，由于 while 的结束条件为 `right == left - 1`，所以你把上述代码中的 `left - 1` 都改成 `right` 也没有问题，这样可能更有利于看出来这是在「搜索右侧边界」。

至此，搜索右侧边界的二分查找的两种写法也完成了，其实将「搜索区间」统一成两端都闭反而更容易记忆，你说是吧？

### 四、逻辑统一

有了搜索左右边界的二分搜索，你可以去解决力扣第 34 题「 [在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)」，

接下来梳理一下这些细节差异的因果逻辑：

**第一个，最基本的二分查找算法**：

```
因为我们初始化 right = nums.length - 1
所以决定了我们的「搜索区间」是 [left, right]
所以决定了 while (left <= right)
同时也决定了 left = mid+1 和 right = mid-1

因为我们只需找到一个 target 的索引即可
所以当 nums[mid] == target 时可以立即返回
```

**第二个，寻找左侧边界的二分查找**：

```
因为我们初始化 right = nums.length
所以决定了我们的「搜索区间」是 [left, right)
所以决定了 while (left < right)
同时也决定了 left = mid + 1 和 right = mid

因为我们需找到 target 的最左侧索引
所以当 nums[mid] == target 时不要立即返回
而要收紧右侧边界以锁定左侧边界
```

**第三个，寻找右侧边界的二分查找**：

```
因为我们初始化 right = nums.length
所以决定了我们的「搜索区间」是 [left, right)
所以决定了 while (left < right)
同时也决定了 left = mid + 1 和 right = mid

因为我们需找到 target 的最右侧索引
所以当 nums[mid] == target 时不要立即返回
而要收紧左侧边界以锁定右侧边界

又因为收紧左侧边界时必须 left = mid + 1
所以最后无论返回 left 还是 right，必须减一
```

对于寻找左右边界的二分搜索，常见的手法是使用左闭右开的「搜索区间」，**我们还根据逻辑将「搜索区间」全都统一成了两端都闭，便于记忆，只要修改两处即可变化出三种写法**：

```
int binary_search(int[] nums, int target) {
    int left = 0, right = nums.length - 1; 
    while(left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1; 
        } else if(nums[mid] == target) {
            // 直接返回
            return mid;
        }
    }
    // 直接返回
    return -1;
}

int left_bound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 别返回，锁定左侧边界
            right = mid - 1;
        }
    }
    // 判断 target 是否存在于 nums 中
    // 此时 target 比所有数都大，返回 -1
    if (left == nums.length) return -1;
    // 判断一下 nums[left] 是不是 target
    return nums[left] == target ? left : -1;
}

int right_bound(int[] nums, int target) {
    int left = 0, right = nums.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 别返回，锁定右侧边界
            left = mid + 1;
        }
    }
    // 此时 left - 1 索引越界
    if (left - 1 < 0) return -1;
    // 判断一下 nums[left] 是不是 target
    return nums[left - 1] == target ? (left - 1) : -1;
}
```

如果以上内容你都能理解，那么恭喜你，二分查找算法的细节不过如此。通过本文，你学会了：

1、分析二分查找代码时，不要出现 else，全部展开成 else if 方便理解。

2、注意「搜索区间」和 while 的终止条件，如果存在漏掉的元素，记得在最后检查。

3、如需定义左闭右开的「搜索区间」搜索左右边界，只要在 `nums[mid] == target` 时做修改即可，搜索右侧时需要减一。

4、如果将「搜索区间」全都统一成两端都闭，好记，只要稍改 `nums[mid] == target` 条件处的代码和返回的逻辑即可，推荐拿小本本记下，作为二分搜索模板。

最后我想说，以上二分搜索的框架属于「术」的范畴，如果上升到「道」的层面，**二分思维的精髓就是：通过已知信息尽可能多地收缩（折半）搜索空间**，从而增加穷举效率，快速找到目标。

理解本文能保证你写出正确的二分查找的代码，但实际题目中不会直接让你写二分代码，我会在 [二分查找的变体](https://appktavsiei5995.pc.xiaoe-tech.com/detail/i_62a07736e4b01a485209b0b4/1) 和 [二分查找的运用](https://labuladong.gitee.io/algo/2/20/31/) 中进一步讲解如何把二分思维运用到更多算法题中。

___

**引用本文的题目**

**安装 [我的 Chrome 刷题插件](https://mp.weixin.qq.com/s/X-fE9sR4BLi6T9pn7xP4pg) 点开下列题目可直接查看解题思路：**

| LeetCode | 力扣 |
| --- | --- |
| [1201\. Ugly Number III](https://leetcode.com/problems/ugly-number-iii/?show=1) | [1201\. 丑数 III](https://leetcode.cn/problems/ugly-number-iii/?show=1) |
| [162\. Find Peak Element](https://leetcode.com/problems/find-peak-element/?show=1) | [162\. 寻找峰值](https://leetcode.cn/problems/find-peak-element/?show=1) |
| [240\. Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/?show=1) | [240\. 搜索二维矩阵 II](https://leetcode.cn/problems/search-a-2d-matrix-ii/?show=1) |
| [33\. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/?show=1) | [33\. 搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/?show=1) |
| [35\. Search Insert Position](https://leetcode.com/problems/search-insert-position/?show=1) | [35\. 搜索插入位置](https://leetcode.cn/problems/search-insert-position/?show=1) |
| [658\. Find K Closest Elements](https://leetcode.com/problems/find-k-closest-elements/?show=1) | [658\. 找到 K 个最接近的元素](https://leetcode.cn/problems/find-k-closest-elements/?show=1) |
| [74\. Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/?show=1) | [74\. 搜索二维矩阵](https://leetcode.cn/problems/search-a-2d-matrix/?show=1) |
| [793\. Preimage Size of Factorial Zeroes Function](https://leetcode.com/problems/preimage-size-of-factorial-zeroes-function/?show=1) | [793\. 阶乘函数后 K 个零](https://leetcode.cn/problems/preimage-size-of-factorial-zeroes-function/?show=1) |
| [81\. Search in Rotated Sorted Array II](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/?show=1) | [81\. 搜索旋转排序数组 II](https://leetcode.cn/problems/search-in-rotated-sorted-array-ii/?show=1) |
| [852\. Peak Index in a Mountain Array](https://leetcode.com/problems/peak-index-in-a-mountain-array/?show=1) | [852\. 山脉数组的峰顶索引](https://leetcode.cn/problems/peak-index-in-a-mountain-array/?show=1) |
| \- | [剑指 Offer 04. 二维数组中的查找](https://leetcode.cn/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/?show=1) |
| \- | [剑指 Offer 53 - I. 在排序数组中查找数字 I](https://leetcode.cn/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/?show=1) |
| \- | [剑指 Offer 53 - II. 0～n-1中缺失的数字](https://leetcode.cn/problems/que-shi-de-shu-zi-lcof/?show=1) |
| \- | [剑指 Offer II 068. 查找插入位置](https://leetcode.cn/problems/N6YdxV/?show=1) |
| \- | [剑指 Offer II 069. 山峰数组的顶部](https://leetcode.cn/problems/B1IidL/?show=1) |

___

**引用本文的文章**

-   [base case 和备忘录的初始值怎么定？](https://labuladong.gitee.io/algo/3/25/72/)
-   [丑数系列算法详解](https://labuladong.github.io/article/fname.html?fname=%e4%b8%91%e6%95%b0)
-   [二分搜索怎么用？我又总结了套路](https://labuladong.gitee.io/algo/2/20/31/)
-   [二分搜索怎么用？我和快手面试官进行了深度探讨](https://labuladong.github.io/article/fname.html?fname=%e4%ba%8c%e5%88%86%e5%88%86%e5%89%b2%e5%ad%90%e6%95%b0%e7%bb%84)
-   [二分搜索算法经典习题](https://appktavsiei5995.pc.xiaoe-tech.com/detail/i_62a07736e4b01a485209b0b4/1)
-   [二分查找高效判定子序列](https://labuladong.gitee.io/algo/4/33/132/)
-   [动态规划设计：最长递增子序列](https://labuladong.gitee.io/algo/3/26/76/)
-   [双指针技巧秒杀七道数组题目](https://labuladong.gitee.io/algo/2/20/23/)
-   [存储系统设计之 LSM 树原理](https://labuladong.gitee.io/algo/5/44/)
-   [带权重的随机选择算法](https://labuladong.gitee.io/algo/2/20/30/)
-   [快速排序详解及应用](https://labuladong.gitee.io/algo/2/21/45/)
-   [我写了首诗，把滑动窗口算法变成了默写题](https://labuladong.gitee.io/algo/2/20/27/)
-   [我的刷题心得](https://labuladong.gitee.io/algo/1/3/)
-   [用算法打败算法](https://labuladong.github.io/article/fname.html?fname=PDF%e4%b8%ad%e7%9a%84%e7%ae%97%e6%b3%95)
-   [讲两道常考的阶乘算法题](https://labuladong.gitee.io/algo/4/32/115/)

___

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

**《labuladong 的算法小抄》已经出版，关注公众号查看详情；后台回复关键词「**进群**」可加入算法群；回复「**全家桶**」可下载配套 PDF 和刷题全家桶**：

[![](https://labuladong.gitee.io/algo/images/souyisou2.png)](https://labuladong.gitee.io/algo/images/souyisou2.png)