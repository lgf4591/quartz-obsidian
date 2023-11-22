---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:34:19
tags: [learn/program/algorithm/labuladong/核心框架]
title: 我的刷题心得  labuladong的算法小抄
---
# 我的刷题心得 Labuladong的算法小抄
## 我的刷题心得

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

**———–**

> 本文有视频版： [学习数据结构和算法的框架思维](https://www.bilibili.com/video/BV1EN4y1M79p/)

两年前刚开这个公众号的时候，我写了一篇 [学习数据结构和算法的框架思维](https://labuladong.gitee.io/algo/1/2/)，现在已经 5w 多阅读了，这对于一篇纯技术文来说是很牛逼的数据。

这两年在我自己不断刷题，思考和写公众号的过程中，我对算法的理解也是在逐渐加深，所以今天再写一篇，把我这两年的经验和思考浓缩成 4000 字，分享给大家。

本文主要有两部分，一是谈我对算法本质的理解，二是概括各种常用的算法。全文没有什么硬核的代码，都是我的经验之谈，也许没有多么高大上，但肯定能帮你少走弯路，更透彻地理解和掌握算法。

如果本文中你有不理解的地方大可跳过，多看一些我的历史文章之后再回过头看，大概就可以明白我想表达的意思了。

另外，本文包含大量历史文章链接，结合本文阅读历史文章也许可以更快培养出学习算法的框架思维和知识体系。

### 算法的本质

**如果要让我一句话总结，我想说算法的本质就是「穷举」**。

这么说肯定有人要反驳了，真的所有算法问题的本质都是穷举吗？没有例外吗？

例外肯定是有的，比如前几天我还发了 [一行代码就能解决的算法题](https://labuladong.gitee.io/algo/4/32/119/)，这些题目类似脑筋急转弯，都是通过观察，发现规律，然后找到最优解法，不过这类算法问题较少，不必特别纠结。再比如，密码学算法、机器学习算法，它们的本质确实不是穷举，而是数学原理的编程实现，所以这类算法的本质是数学，不在我们所探讨的「数据结构和算法」的范畴之内。

**顺便强调下，「算法工程师」做的这个「算法」，和「数据结构与算法」中的这个「算法」完全是两码事**，免得一些初学同学误解。

对前者来说，重点在数学建模和调参经验，计算机真就只是拿来做计算的工具而已；而后者的重点是计算机思维，需要你能够站在计算机的视角，抽象、化简实际问题，然后用合理的数据结构去解决问题。

所以，你千万别以为学好了数据结构和算法就能去做算法工程师，也不要以为只要不做算法工程师就不需要学习数据结构和算法。坦白说，大部分开发岗位工作中都是基于现成的开发框架做事，不怎么会碰到底层数据结构和算法相关的问题，但另一个事实是，只要你想找技术相关的岗位，数据结构和算法的考察是绕不开的，因为这块知识点是公认的程序员基本功。

**为了区分，不妨称算法工程师研究的算法为「数学算法」，称刷题面试的算法为「计算机算法」，我写的内容主要聚焦的是「计算机算法」**。

这样解释应该很清楚了吧，我猜大部分人的目标是通过算法笔试，找一份开发岗位的工作，所以你真的不需要有多少数学基础，只要学会用计算机思维解决问题就够了。

其实计算机思维也没什么高端的，你想想计算机的特点是啥？不就是快嘛，你的脑回路一秒只能转一圈，人家 CPU 转几万圈无压力。所以计算机解决问题的方式大道至简，就是穷举。

我记得自己刚入门的时候，也觉得计算机算法是一个很高大上的东西，每见到一道题，就想着能不能推导出一个什么数学公式，啪的一下就能把答案算出来。

比如你和一个没学过计算机算法的人说你写了个计算排列组合的算法，他大概以为你发明了一个公式，可以直接算出所有排列组合。但实际上呢？没什么高大上的公式，后文 [回溯算法秒杀排列组合子集问题](https://labuladong.gitee.io/algo/4/31/106/) 写了，其实就是把排列组合问题抽象成一棵多叉树结构，然后用回溯算法去暴力穷举。

对计算机算法的误解也许是以前学数学留下的「后遗症」，数学题一般都是你仔细观察，找几何关系，列方程，然后算出答案。如果说你需要进行大规模穷举来寻找答案，那大概率是你的解题思路出问题了。

而计算机解决问题的思维恰恰相反：有没有什么数学公式就交给你们人类去推导吧，如果能找到一些巧妙的定理那最好，但如果找不到，那就穷举呗，反正只要复杂度允许，没有什么答案是穷举不出来的，理论上讲只要不断随机打乱一个数组，总有一天能得到有序的结果呢！当然，这绝不是一个好算法，因为鬼知道它要运行多久才有结果。

技术岗笔试面试考的那些算法题，求个最大值最小值什么的，你怎么求？必须得把所有可行解穷举出来才能找到最值对吧，说白了不就这么点事儿么。

**但是，你千万不要觉得穷举这个事儿很简单，穷举有两个关键难点：无遗漏、无冗余**。

遗漏，会直接导致答案出错；冗余，会拖慢算法的运行速度。所以，当你看到一道算法题，可以从这两个维度去思考：

**1、如何穷举**？即无遗漏地穷举所有可能解。

**2、如何聪明地穷举**？即避免所有冗余的计算，消耗尽可能少的资源求出答案。

不同类型的题目，难点是不同的，有的题目难在「如何穷举」，有的题目难在「如何聪明地穷举」。

**什么算法的难点在「如何穷举」呢？一般是递归类问题，最典型的就是动态规划系列问题**。

后文 [动态规划核心套路](https://labuladong.gitee.io/algo/3/25/69/) 阐述了动态规划系列问题的核心原理，无非就是先写出暴力穷举解法（状态转移方程），加个备忘录就成自顶向下的递归解法了，再改一改就成自底向上的递推迭代解法了， [动态规划的降维打击](https://labuladong.gitee.io/algo/3/25/73/) 里也讲过如何分析优化动态规划算法的空间复杂度。

上述过程就是在不断优化算法的时间、空间复杂度，也就是所谓「如何聪明地穷举」。这些技巧一听就会了，但很多读者留言说明白了这些原理，遇到动态规划题目还是不会做，因为想不出状态转移方程，第一步的暴力解法都写不出来。

这很正常，因为动态规划类型的题目可以千奇百怪，找状态转移方程才是难点，所以才有了 [动态规划设计方法：数学归纳法](https://labuladong.gitee.io/algo/3/26/76/) 这篇文章，告诉你递归穷举的核心是数学归纳法，明确函数的定义，然后利用这个定义写递归函数，就可以穷举出所有可行解。

**什么算法的难点在「如何聪明地穷举」呢？一些耳熟能详的非递归算法技巧，都可以归在这一类**。

比如后文 [Union Find 并查集算法详解](https://labuladong.gitee.io/algo/2/22/53/) 告诉你一种高效计算连通分量的技巧，理论上说，想判断两个节点是否连通，我用 DFS/BFS 暴力搜索（穷举）肯定可以做到，但人家 Union Find 算法硬是用数组模拟树结构，给你把连通性相关的操作复杂度给干到 `O(1)` 了。

这就属于聪明地穷举，大佬们把这些技巧发明出来，你学过就会用，没学过恐怕很难想出这种思路。

再比如贪心算法技巧，后文 [当老司机学会贪心算法](https://labuladong.github.io/article/fname.html?fname=%e8%80%81%e5%8f%b8%e6%9c%ba) 就告诉你，所谓贪心算法就是在题目中发现一些规律（专业点叫贪心选择性质），使得你不用完整穷举所有解就可以得出答案。

人家动态规划好歹是无冗余地穷举所有解，然后找一个最值，你贪心算法可好，都不用穷举所有解就可以找到答案，所以后文 [贪心算法解决跳跃游戏](https://labuladong.gitee.io/algo/3/29/102/) 中贪心算法的效率比动态规划还高。

再比如大名鼎鼎的 KMP 算法，你写个字符串暴力匹配算法很容易，但你发明个 KMP 算法试试？KMP 算法的本质是聪明地缓存并复用一些信息，减少了冗余计算，后文 [KMP 字符匹配算法](https://labuladong.gitee.io/algo/3/28/97/) 就是使用状态机的思路实现的 KMP 算法。

下面我概括性地列举一些常见的算法技巧，供大家学习参考。

### 数组/单链表系列算法

**单链表常考的技巧就是双指针**，后文 [单链表六大技巧](https://labuladong.gitee.io/algo/2/19/18/) 全给你总结好了，这些技巧就是会者不难，难者不会。

比如判断单链表是否成环，拍脑袋的暴力解是什么？就是用一个 `HashSet` 之类的数据结构来缓存走过的节点，遇到重复的就说明有环对吧。但我们用快慢指针可以避免使用额外的空间，这就是聪明地穷举嘛。

当然，对于找链表中点这种问题，使用双指针技巧只是显示你学过这个技巧，和遍历两次链表的常规解法从时间空间复杂度的角度来说都是差不多的。

**数组常用的技巧有很大一部分还是双指针相关的技巧，说白了是教你如何聪明地进行穷举**。

**首先说二分搜索技巧**，可以归为两端向中心的双指针。如果让你在数组中搜索元素，一个 for 循环穷举肯定能搞定对吧，但如果数组是有序的，二分搜索不就是一种更聪明的搜索方式么。

后文 [二分搜索框架详解](https://labuladong.gitee.io/algo/2/20/29/) 给你总结了二分搜索代码模板，保证不会出现搜索边界的问题。后文 [二分搜索算法运用](https://labuladong.gitee.io/algo/2/20/31/) 给你总结了二分搜索相关题目的共性以及如何将二分搜索思想运用到实际算法中。

类似的两端向中心的双指针技巧还有力扣上的 N 数之和系列问题，后文 [一个函数秒杀所有 nSum 问题](https://labuladong.gitee.io/algo/1/15/) 讲了这些题目的共性，甭管几数之和，解法肯定要穷举所有的数字组合，然后看看那个数字组合的和等于目标和嘛。比较聪明的方式是先排序，利用双指针技巧快速计算结果。

**再说说 [滑动窗口算法技巧](https://labuladong.gitee.io/algo/2/20/27/)**，典型的快慢双指针，快慢指针中间就是滑动的「窗口」，主要用于解决子串问题。

文中最小覆盖子串这道题，让你寻找包含特定字符的最短子串，常规拍脑袋解法是什么？那肯定是类似字符串暴力匹配算法，用嵌套 for 循环穷举呗，平方级的复杂度。

而滑动窗口技巧告诉你不用这么麻烦，可以用快慢指针遍历一次就求出答案，这就是教你聪明的穷举技巧。

但是，就好像二分搜索只能运用在有序数组上一样，滑动窗口也是有其限制的，就是你必须明确的知道什么时候应该扩大窗口，什么时候该收缩窗口。

比如后文 [最大子数组问题](https://labuladong.gitee.io/algo/3/26/77/) 面对的问题就没办法用滑动窗口，因为数组中的元素存在负数，扩大或缩小窗口并不能保证窗口中的元素之和就会随着增大和减小，所以无法使用滑动窗口技巧，只能用动态规划技巧穷举了。

**还有回文串相关技巧**，如果判断一个串是否是回文串，使用双指针从两端向中心检查，如果寻找回文子串，就从中心向两端扩散。后文 [最长回文子串](https://labuladong.gitee.io/algo/2/20/23/) 使用了一种技巧同时处理了回文串长度为奇数或偶数的情况。

当然，寻找最长回文子串可以有更精妙的马拉车算法（Manacher 算法），不过，学习这个算法的性价比不高，没什么必要掌握。

**最后说说 [前缀和技巧](https://labuladong.gitee.io/algo/2/20/24/) 和 [差分数组技巧](https://labuladong.gitee.io/algo/2/20/25/)**。

如果频繁地让你计算子数组的和，每次用 for 循环去遍历肯定没问题，但前缀和技巧预计算一个 `preSum` 数组，就可以避免循环。

类似的，如果频繁地让你对子数组进行增减操作，也可以每次用 for 循环去操作，但差分数组技巧维护一个 `diff` 数组，也可以避免循环。

数组链表的技巧差不多就这些了，都比较固定，只要你都见过，运用出来的难度不算大，下面来说一说稍微有些难度的算法。

### 二叉树系列算法

老读者都知道，二叉树的重要性我之前说了无数次，因为二叉树模型几乎是所有高级算法的基础，尤其是那么多人说对递归的理解不到位，更应该好好刷二叉树相关题目。

PS： [刷题插件](https://mp.weixin.qq.com/s/OE1zPVPj0V2o82N4HtLQbw) 集成了手把手刷二叉树功能，按照公式和套路讲解了 150 道二叉树题目，可手把手带你刷完二叉树分类的题目，迅速掌握递归思维。

**[东哥带你刷二叉树（纲领篇）](https://labuladong.gitee.io/algo/2/21/36/) 说过，二叉树题目的递归解法可以分两类思路，第一类是遍历一遍二叉树得出答案，第二类是通过分解问题计算出答案，这两类思路分别对应着 [回溯算法核心框架](https://labuladong.gitee.io/algo/4/31/104/) 和 [动态规划核心框架](https://labuladong.gitee.io/algo/3/25/69/)**。

**什么叫通过遍历一遍二叉树得出答案**？

就比如说计算二叉树最大深度这个问题让你实现 `maxDepth` 这个函数，你这样写代码完全没问题：

```
// 记录最大深度
int res = 0;
int depth = 0;

// 主函数
int maxDepth(TreeNode root) {
traverse(root);
return res;
}

// 二叉树遍历框架
void traverse(TreeNode root) {
if (root == null) {
// 到达叶子节点
res = Math.max(res, depth);
return;
}
// 前序遍历位置
depth++;
traverse(root.left);
traverse(root.right);
// 后序遍历位置
depth--;
}
```

这个逻辑就是用 `traverse` 函数遍历了一遍二叉树的所有节点，维护 `depth` 变量，在叶子节点的时候更新最大深度。

你看这段代码，有没有觉得很熟悉？能不能和回溯算法的代码模板对应上？

不信你照着 [回溯算法核心框架](https://labuladong.gitee.io/algo/4/31/104/) 中全排列问题的代码对比下：

```
// 记录所有全排列
List<List<Integer>> res = new LinkedList<>();
LinkedList<Integer> track = new LinkedList<>();

/* 主函数，输入一组不重复的数字，返回它们的全排列 */
List<List<Integer>> permute(int[] nums) {
    backtrack(nums);
    return res;
}

// 回溯算法框架
void backtrack(int[] nums) {
    if (track.size() == nums.length) {
// 穷举完一个全排列
        res.add(new LinkedList(track));
        return;
    }
    
    for (int i = 0; i < nums.length; i++) {
        if (track.contains(nums[i]))
            continue;
// 前序遍历位置做选择
        track.add(nums[i]);
        backtrack(nums);
        // 后序遍历位置取消选择
        track.removeLast();
    }
}
```

前文讲回溯算法的时候就告诉你回溯算法本质就是遍历一棵多叉树，连代码实现都如出一辙有没有？

而且我之前经常说，回溯算法虽然简单粗暴效率低，但特别有用，因为如果你对一道题无计可施，回溯算法起码能帮你写一个暴力解捞点分对吧。

**那什么叫通过分解问题计算答案**？

同样是计算二叉树最大深度这个问题，你也可以写出下面这样的解法：

```
// 定义：输入根节点，返回这棵二叉树的最大深度
int maxDepth(TreeNode root) {
if (root == null) {
return 0;
}
// 递归计算左右子树的最大深度
int leftMax = maxDepth(root.left);
int rightMax = maxDepth(root.right);
// 整棵树的最大深度
int res = Math.max(leftMax, rightMax) + 1;

return res;
}
```

你看这段代码，有没有觉得很熟悉？有没有觉得有点动态规划解法代码的形式？

不信你看 [动态规划核心框架](https://labuladong.gitee.io/algo/3/25/69/) 中凑零钱问题的暴力穷举解法：

```
// 定义：输入金额 amount，返回凑出 amount 的最少硬币个数
int coinChange(int[] coins, int amount) {
    // base case
    if (amount == 0) return 0;
    if (amount < 0) return -1;

    int res = Integer.MAX_VALUE;
    for (int coin : coins) {
        // 递归计算凑出 amount - coin 的最少硬币个数
        int subProblem = coinChange(coins, amount - coin);
        if (subProblem == -1) continue;
        // 凑出 amount 的最少硬币个数
        res = Math.min(res, subProblem + 1);
    }

    return res == Integer.MAX_VALUE ? -1 : res;
}
```

这个暴力解法加个 `memo` 备忘录就是自顶向下的动态规划解法，你对照二叉树最大深度的解法代码，有没有发现很像？

**如果你感受到最大深度这个问题两种解法的区别，那就趁热打铁，我问你，二叉树的前序遍历怎么写**？

我相信大家都会对这个问题嗤之以鼻，毫不犹豫就可以写出下面这段代码：

```
List<Integer> res = new LinkedList<>();

// 返回前序遍历结果
List<Integer> preorder(TreeNode root) {
    traverse(root);
    return res;
}

// 二叉树遍历函数
void traverse(TreeNode root) {
    if (root == null) {
        return;
    }
    // 前序遍历位置
    res.add(root.val);
    traverse(root.left);
    traverse(root.right);
}
```

但是，你结合上面说到的两种不同的思维模式，二叉树的遍历是否也可以通过分解问题的思路解决呢？

我们后文 [手把手刷二叉树（第二期）](https://labuladong.gitee.io/algo/2/21/38/) 说过前中后序遍历结果的特点：

[![](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%8f%89%e6%a0%91%e7%b3%bb%e5%88%972/1.jpeg)](https://labuladong.gitee.io/algo/images/%e4%ba%8c%e5%8f%89%e6%a0%91%e7%b3%bb%e5%88%972/1.jpeg)

**你注意前序遍历的结果，根节点的值在第一位，后面接着左子树的前序遍历结果，最后接着右子树的前序遍历结果**。

有没有体会出点什么来？其实完全可以重写前序遍历代码，用分解问题的形式写出来，避免外部变量和辅助函数：

```
// 定义：输入一棵二叉树的根节点，返回这棵树的前序遍历结果
List<Integer> preorder(TreeNode root) {
    List<Integer> res = new LinkedList<>();
    if (root == null) {
        return res;
    }
    // 前序遍历的结果，root.val 在第一个
    res.add(root.val);
    // 后面接着左子树的前序遍历结果
    res.addAll(preorder(root.left));
    // 最后接着右子树的前序遍历结果
    res.addAll(preorder(root.right));
    return res;
}
```

你看，这就是用分解问题的思维模式写二叉树的前序遍历，如果写中序和后序遍历也是类似的。

当然，动态规划系列问题有「最优子结构」和「重叠子问题」两个特性，而且大多是让你求最值的。很多算法虽然不属于动态规划，但也符合分解问题的思维模式。

比如 [分治算法详解](https://labuladong.gitee.io/algo/4/33/122/) 中说到的运算表达式优先级问题，其核心依然是大问题分解成子问题，只不过没有重叠子问题，不能用备忘录去优化效率罢了。

当然，除了动归、回溯（DFS）、分治，还有一个常用算法就是 BFS 了，后文 [BFS 算法核心框架](https://labuladong.gitee.io/algo/4/31/110/) 就是根据下面这段二叉树的层序遍历代码改装出来的：

```
// 输入一棵二叉树的根节点，层序遍历这棵二叉树
void levelTraverse(TreeNode root) {
    if (root == null) return 0;
    Queue<TreeNode> q = new LinkedList<>();
    q.offer(root);

    int depth = 1;
    // 从上到下遍历二叉树的每一层
    while (!q.isEmpty()) {
        int sz = q.size();
        // 从左到右遍历每一层的每个节点
        for (int i = 0; i < sz; i++) {
            TreeNode cur = q.poll();

            if (cur.left != null) {
                q.offer(cur.left);
            }
            if (cur.right != null) {
                q.offer(cur.right);
            }
        }
        depth++;
    }
}
```

**更进一步，图论相关的算法也是二叉树算法的延续**。

比如 [图论基础](https://labuladong.gitee.io/algo/2/22/50/)， [环判断和拓扑排序](https://labuladong.gitee.io/algo/2/22/51/) 和 [二分图判定算法](https://labuladong.gitee.io/algo/2/22/52/) 就用到了 DFS 算法；再比如 [Dijkstra 算法模板](https://labuladong.gitee.io/algo/2/22/56/)，就是改造版 BFS 算法加上一个类似 dp table 的数组。

好了，说的差不多了，上述这些算法的本质都是穷举二（多）叉树，有机会的话通过剪枝或者备忘录的方式减少冗余计算，提高效率，就这么点事儿。

### 最后总结

上周在视频号直播的时候，有读者问我什么刷题方式是正确的，我说正确的刷题方式应该是刷一道题能获得刷十道题的效果，不然力扣现在 2000 道题目，你都打算刷完么？

那么怎么做到呢？ [学习数据结构和算法的框架思维](https://labuladong.gitee.io/algo/1/2/) 说了，要有框架思维，学会提炼重点，一个算法技巧可以包装出一百道题，如果你能一眼看穿它的本质，那就没必要浪费时间刷了嘛。

同时，在做题的时候要思考，联想，进而培养举一反三的能力。

后文 [Dijkstra 算法模板](https://labuladong.gitee.io/algo/2/22/56/) 并不是真的是让你去背代码模板，不然的话直接甩出来那一段代码不就行了，我从层序遍历讲到 BFS 讲到 Dijkstra，说这么多废话干什么？

说到底我还是希望爱思考的读者能培养出成体系的算法思维，最好能爱上算法，而不是单纯地看题解去做题，授人以鱼不如授人以渔嘛。

本文就到这里吧，**算法真的没啥难的，只要有心，谁都可以学好**。分享是一种美德，如果本文对你有启发，欢迎分享给需要的朋友。

> 最后打个广告，我亲自制作了一门 [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO)，以视频课为主，手把手带你实现常用的数据结构及相关算法，旨在帮助算法基础较为薄弱的读者深入理解常用数据结构的底层原理，在算法学习中少走弯路。

___

**引用本文的题目**

**安装 [我的 Chrome 刷题插件](https://mp.weixin.qq.com/s/X-fE9sR4BLi6T9pn7xP4pg) 点开下列题目可直接查看解题思路：**

| LeetCode | 力扣 |
| --- | --- |
| [104\. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/?show=1) | [104\. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/?show=1) |
| [112\. Path Sum](https://leetcode.com/problems/path-sum/?show=1) | [112\. 路径总和](https://leetcode.cn/problems/path-sum/?show=1) |
| [117\. Populating Next Right Pointers in Each Node II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/?show=1) | [117\. 填充每个节点的下一个右侧节点指针 II](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node-ii/?show=1) |
| [1214\. Two Sum BSTs](https://leetcode.com/problems/two-sum-bsts/?show=1)🔒 | [1214\. 查找两棵二叉搜索树之和](https://leetcode.cn/problems/two-sum-bsts/?show=1)🔒 |
| [395\. Longest Substring with At Least K Repeating Characters](https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/?show=1) | [395\. 至少有 K 个重复字符的最长子串](https://leetcode.cn/problems/longest-substring-with-at-least-k-repeating-characters/?show=1) |
| [653\. Two Sum IV - Input is a BST](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/?show=1) | [653\. 两数之和 IV - 输入 BST](https://leetcode.cn/problems/two-sum-iv-input-is-a-bst/?show=1) |
| [94\. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/?show=1) | [94\. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/?show=1) |
| \- | [剑指 Offer 55 - I. 二叉树的深度](https://leetcode.cn/problems/er-cha-shu-de-shen-du-lcof/?show=1) |
| \- | [剑指 Offer II 056. 二叉搜索树中两个节点之和](https://leetcode.cn/problems/opLdQZ/?show=1) |

___

**引用本文的文章**

-   [东哥带你刷二叉树（纲领篇）](https://labuladong.gitee.io/algo/2/21/36/)
-   [动态规划问题的两种穷举视角](https://labuladong.github.io/article/fname.html?fname=%e5%8a%a8%e5%bd%92%e4%b8%a4%e7%a7%8d%e8%a7%86%e8%a7%92)
-   [算法学习和心流体验](https://labuladong.github.io/article/fname.html?fname=%e5%bf%83%e6%b5%81)

___

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

**《labuladong 的算法小抄》已经出版，关注公众号查看详情；后台回复关键词「**进群**」可加入算法群；回复「**全家桶**」可下载配套 PDF 和刷题全家桶**：

[![](https://labuladong.gitee.io/algo/images/souyisou2.png)](https://labuladong.gitee.io/algo/images/souyisou2.png)