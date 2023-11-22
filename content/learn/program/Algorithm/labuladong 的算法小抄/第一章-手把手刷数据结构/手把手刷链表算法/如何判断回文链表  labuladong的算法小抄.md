---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:24:56
tags: [learn/program/algorithm/labuladong/数据结构/链表]
title: 如何判断回文链表  labuladong的算法小抄
---
# 如何判断回文链表 Labuladong的算法小抄
## 如何判断回文链表

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

读完本文，你不仅学会了算法套路，还可以顺便解决如下题目：

| LeetCode | 力扣 | 难度 |
| --- | --- | --- |
| [234\. Palindrome Linked List](https://leetcode.com/problems/palindrome-linked-list/) | [234\. 回文链表](https://leetcode.cn/problems/palindrome-linked-list/) | 🟢 |
| \- | [剑指 Offer II 027. 回文链表](https://leetcode.cn/problems/aMhZSa/) | 🟢 |

**———–**

前文 [数组双指针技巧汇总](https://labuladong.gitee.io/algo/2/20/23/) 和 [子序列问题解题思路](https://labuladong.gitee.io/algo/3/26/79/) 讲解了回文串和回文序列相关的问题，先来简单回顾下。

**寻找**回文串的核心思想是从中心向两端扩展：

```
// 在 s 中寻找以 s[left] 和 s[right] 为中心的最长回文串
String palindrome(String s, int left, int right) {
    // 防止索引越界
    while (left >= 0 && right < s.length()
            && s.charAt(left) == s.charAt(right)) {
        // 双指针，向两边展开
        left--;
        right++;
    }
    // 返回以 s[left] 和 s[right] 为中心的最长回文串
    return s.substring(left + 1, right);
}
```

因为回文串长度可能为奇数也可能是偶数，长度为奇数时只存在一个中心点，而长度为偶数时存在两个中心点，所以上面这个函数需要传入 `l` 和 `r`。

而**判断**一个字符串是不是回文串就简单很多，不需要考虑奇偶情况，只需要 [双指针技巧](https://labuladong.gitee.io/algo/2/20/23/)，从两端向中间逼近即可：

```
boolean isPalindrome(String s) {
    // 一左一右两个指针相向而行
    int left = 0, right = s.length() - 1;
    while (left < right) {
        if (s.charAt(left) != s.charAt(right)) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}
```

以上代码很好理解吧，**因为回文串是对称的，所以正着读和倒着读应该是一样的，这一特点是解决回文串问题的关键**。

下面扩展这一最简单的情况，来解决：如何判断一个「单链表」是不是回文。

### 一、判断回文单链表

看下力扣第 234 题「 [回文链表](https://leetcode.cn/problems/palindrome-linked-list/)」：

输入一个单链表的头结点，判断这个链表中的数字是不是回文，函数签名如下：

```
boolean isPalindrome(ListNode head);
```

比如说：

```
输入: 1->2->null
输出: false

输入: 1->2->2->1->null
输出: true
```

这道题的关键在于，单链表无法倒着遍历，无法使用双指针技巧。

那么最简单的办法就是，把原始链表反转存入一条新的链表，然后比较这两条链表是否相同。关于如何反转链表，可以参见前文 [递归翻转链表的一部分](https://labuladong.gitee.io/algo/2/19/19/)。

其实，**借助二叉树后序遍历的思路，不需要显式反转原始链表也可以倒序遍历链表**，下面来具体聊聊。

对于二叉树的几种遍历方式，我们再熟悉不过了：

```
void traverse(TreeNode root) {
    // 前序遍历代码
    traverse(root.left);
    // 中序遍历代码
    traverse(root.right);
    // 后序遍历代码
}
```

在 [学习数据结构的框架思维](https://labuladong.gitee.io/algo/1/2/) 中说过，链表兼具递归结构，树结构不过是链表的衍生。那么，**链表其实也可以有前序遍历和后序遍历**：

```
void traverse(ListNode head) {
    // 前序遍历代码
    traverse(head.next);
    // 后序遍历代码
}
```

这个框架有什么指导意义呢？如果我想正序打印链表中的 `val` 值，可以在前序遍历位置写代码；反之，如果想倒序遍历链表，就可以在后序遍历位置操作：

```
/* 倒序打印单链表中的元素值 */
void traverse(ListNode head) {
    if (head == null) return;
    traverse(head.next);
    // 后序遍历代码
    print(head.val);
}
```

说到这了，其实可以稍作修改，模仿双指针实现回文判断的功能：

```
// 左侧指针
ListNode left;

boolean isPalindrome(ListNode head) {
    left = head;
    return traverse(head);
}

boolean traverse(ListNode right) {
    if (right == null) return true;
    boolean res = traverse(right.next);
    // 后序遍历代码
    res = res && (right.val == left.val);
    left = left.next;
    return res;
}
```

这么做的核心逻辑是什么呢？**实际上就是把链表节点放入一个栈，然后再拿出来，这时候元素顺序就是反的**，只不过我们利用的是递归函数的堆栈而已，如下 GIF 所示：

[![](https://labuladong.gitee.io/algo/images/%e5%9b%9e%e6%96%87%e9%93%be%e8%a1%a8/1.gif)](https://labuladong.gitee.io/algo/images/%e5%9b%9e%e6%96%87%e9%93%be%e8%a1%a8/1.gif)

当然，无论造一条反转链表还是利用后序遍历，算法的时间和空间复杂度都是 O(N)。下面我们想想，能不能不用额外的空间，解决这个问题呢？

### 二、优化空间复杂度

更好的思路是这样的：

**1、先通过 [双指针技巧](https://labuladong.gitee.io/algo/2/19/18/) 中的快慢指针来找到链表的中点**：

```
ListNode slow, fast;
slow = fast = head;
while (fast != null && fast.next != null) {
    slow = slow.next;
    fast = fast.next.next;
}
// slow 指针现在指向链表中点
```

[![](https://labuladong.gitee.io/algo/images/%e5%9b%9e%e6%96%87%e9%93%be%e8%a1%a8/1.jpg)](https://labuladong.gitee.io/algo/images/%e5%9b%9e%e6%96%87%e9%93%be%e8%a1%a8/1.jpg)

**2、如果`fast`指针没有指向`null`，说明链表长度为奇数，`slow`还要再前进一步**：

```
if (fast != null)
    slow = slow.next;
```

[![](https://labuladong.gitee.io/algo/images/%e5%9b%9e%e6%96%87%e9%93%be%e8%a1%a8/2.jpg)](https://labuladong.gitee.io/algo/images/%e5%9b%9e%e6%96%87%e9%93%be%e8%a1%a8/2.jpg)

**3、从`slow`开始反转后面的链表，现在就可以开始比较回文串了**：

```
ListNode left = head;
ListNode right = reverse(slow);

while (right != null) {
    if (left.val != right.val)
        return false;
    left = left.next;
    right = right.next;
}
return true;
```

[![](https://labuladong.gitee.io/algo/images/%e5%9b%9e%e6%96%87%e9%93%be%e8%a1%a8/3.jpg)](https://labuladong.gitee.io/algo/images/%e5%9b%9e%e6%96%87%e9%93%be%e8%a1%a8/3.jpg)

至此，把上面 3 段代码合在一起就高效地解决这个问题了，其中 `reverse` 函数很容易实现：

```
boolean isPalindrome(ListNode head) {
    ListNode slow, fast;
    slow = fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }
    
    if (fast != null)
        slow = slow.next;
    
    ListNode left = head;
    ListNode right = reverse(slow);
    while (right != null) {
        if (left.val != right.val)
            return false;
        left = left.next;
        right = right.next;
    }
    
    return true;
}

ListNode reverse(ListNode head) {
    ListNode pre = null, cur = head;
    while (cur != null) {
        ListNode next = cur.next;
        cur.next = pre;
        pre = cur;
        cur = next;
    }
    return pre;
}
```

算法过程如下 GIF 所示：

[![](https://labuladong.gitee.io/algo/images/kgroup/8.gif)](https://labuladong.gitee.io/algo/images/kgroup/8.gif)

算法总体的时间复杂度 O(N)，空间复杂度 O(1)，已经是最优的了。

我知道肯定有读者会问：这种解法虽然高效，但破坏了输入链表的原始结构，能不能避免这个瑕疵呢？

其实这个问题很好解决，关键在于得到`p, q`这两个指针位置：

[![](https://labuladong.gitee.io/algo/images/%e5%9b%9e%e6%96%87%e9%93%be%e8%a1%a8/4.jpg)](https://labuladong.gitee.io/algo/images/%e5%9b%9e%e6%96%87%e9%93%be%e8%a1%a8/4.jpg)

这样，只要在函数 return 之前加一段代码即可恢复原先链表顺序：

篇幅所限，我就不写了，读者可以自己尝试一下。

### 三、最后总结

首先，寻找回文串是从中间向两端扩展，判断回文串是从两端向中间收缩。对于单链表，无法直接倒序遍历，可以造一条新的反转链表，可以利用链表的后序遍历，也可以用栈结构倒序处理单链表。

具体到回文链表的判断问题，由于回文的特殊性，可以不完全反转链表，而是仅仅反转部分链表，将空间复杂度降到 O(1)。

> 最后打个广告，我亲自制作了一门 [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO)，以视频课为主，手把手带你实现常用的数据结构及相关算法，旨在帮助算法基础较为薄弱的读者深入理解常用数据结构的底层原理，在算法学习中少走弯路。

___

**引用本文的题目**

**安装 [我的 Chrome 刷题插件](https://mp.weixin.qq.com/s/X-fE9sR4BLi6T9pn7xP4pg) 点开下列题目可直接查看解题思路：**

| LeetCode | 力扣 |
| --- | --- |
| \- | [剑指 Offer II 027. 回文链表](https://leetcode.cn/problems/aMhZSa/?show=1) |

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

**《labuladong 的算法小抄》已经出版，关注公众号查看详情；后台回复关键词「**进群**」可加入算法群；回复「**全家桶**」可下载配套 PDF 和刷题全家桶**：

[![](https://labuladong.gitee.io/algo/images/souyisou2.png)](https://labuladong.gitee.io/algo/images/souyisou2.png)