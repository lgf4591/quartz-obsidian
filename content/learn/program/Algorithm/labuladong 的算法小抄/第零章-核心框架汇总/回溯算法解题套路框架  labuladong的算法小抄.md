---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:34:01
tags: [learn/program/algorithm/labuladong/核心框架]
title: 回溯算法解题套路框架  labuladong的算法小抄
---
# 回溯算法解题套路框架 Labuladong的算法小抄
## 回溯算法解题套路框架

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

读完本文，你不仅学会了算法套路，还可以顺便解决如下题目：

| LeetCode | 力扣 | 难度 |
| --- | --- | --- |
| [46\. Permutations](https://leetcode.com/problems/permutations/) | [46\. 全排列](https://leetcode.cn/problems/permutations/) | 🟠 |
| [51\. N-Queens](https://leetcode.com/problems/n-queens/) | [51\. N 皇后](https://leetcode.cn/problems/n-queens/) | 🔴 |
| \- | [剑指 Offer II 083. 没有重复元素集合的全排列](https://leetcode.cn/problems/VvJkup/) | 🟠 |

**———–**

> 本文有视频版： [回溯算法框架套路详解](https://www.bilibili.com/video/BV1P5411N7Xc/)

这篇文章是很久之前的一篇 [回溯算法详解](https://mp.weixin.qq.com/s/trILKSiN9EoS58pXmvUtUQ) 的进阶版，之前那篇不够清楚，就不必看了，看这篇就行。把框架给你讲清楚，你会发现回溯算法问题都是一个套路。

本文解决几个问题：

回溯算法是什么？解决回溯算法相关的问题有什么技巧？如何学习回溯算法？回溯算法代码是否有规律可循？

其实回溯算法和我们常说的 DFS 算法非常类似，本质上就是一种暴力穷举算法。回溯算法和 DFS 算法的细微差别是：**回溯算法是在遍历「树枝」，DFS 算法是在遍历「节点」**，本文就是简单提一下，等你看到后文 [图论算法基础](https://labuladong.gitee.io/algo/2/22/50/) 时就能深刻理解这句话的含义了。

废话不多说，直接上回溯算法框架，解决一个回溯问题，实际上就是一个决策树的遍历过程，站在回溯树的一个节点上，你只需要思考 3 个问题：

1、路径：也就是已经做出的选择。

2、选择列表：也就是你当前可以做的选择。

3、结束条件：也就是到达决策树底层，无法再做选择的条件。

如果你不理解这三个词语的解释，没关系，我们后面会用「全排列」和「N 皇后问题」这两个经典的回溯算法问题来帮你理解这些词语是什么意思，现在你先留着印象。

代码方面，回溯算法的框架：

```
result = []
def backtrack(路径, 选择列表):
    if 满足结束条件:
        result.add(路径)
        return
    
    for 选择 in 选择列表:
        做选择
        backtrack(路径, 选择列表)
        撤销选择
```

**其核心就是 for 循环里面的递归，在递归调用之前「做选择」，在递归调用之后「撤销选择」**，特别简单。

什么叫做选择和撤销选择呢，这个框架的底层原理是什么呢？下面我们就通过「全排列」这个问题来解开之前的疑惑，详细探究一下其中的奥妙！

### 一、全排列问题

力扣第 46 题「 [全排列](https://leetcode.cn/problems/permutations/)」就是给你输入一个数组 `nums`，让你返回这些数字的全排列。

> **PS：我们这次讨论的全排列问题不包含重复的数字，包含重复数字的扩展场景我在后文 [回溯算法秒杀排列组合子集的九种题型](https://labuladong.gitee.io/algo/4/31/106/) 中讲解**。

我们在高中的时候就做过排列组合的数学题，我们也知道 `n` 个不重复的数，全排列共有 `n!` 个。那么我们当时是怎么穷举全排列的呢？

比方说给三个数 `[1,2,3]`，你肯定不会无规律地乱穷举，一般是这样：

先固定第一位为 1，然后第二位可以是 2，那么第三位只能是 3；然后可以把第二位变成 3，第三位就只能是 2 了；然后就只能变化第一位，变成 2，然后再穷举后两位……

其实这就是回溯算法，我们高中无师自通就会用，或者有的同学直接画出如下这棵回溯树：

[![](https://labuladong.gitee.io/algo/images/backtracking/1.jpg)](https://labuladong.gitee.io/algo/images/backtracking/1.jpg)

只要从根遍历这棵树，记录路径上的数字，其实就是所有的全排列。**我们不妨把这棵树称为回溯算法的「决策树」**。

**为啥说这是决策树呢，因为你在每个节点上其实都在做决策**。比如说你站在下图的红色节点上：

[![](https://labuladong.gitee.io/algo/images/backtracking/2.jpg)](https://labuladong.gitee.io/algo/images/backtracking/2.jpg)

你现在就在做决策，可以选择 1 那条树枝，也可以选择 3 那条树枝。为啥只能在 1 和 3 之中选择呢？因为 2 这个树枝在你身后，这个选择你之前做过了，而全排列是不允许重复使用数字的。

**现在可以解答开头的几个名词：`[2]` 就是「路径」，记录你已经做过的选择；`[1,3]` 就是「选择列表」，表示你当前可以做出的选择；「结束条件」就是遍历到树的底层叶子节点，这里也就是选择列表为空的时候**。

如果明白了这几个名词，可以把「路径」和「选择」列表作为决策树上每个节点的属性，比如下图列出了几个蓝色节点的属性：

[![](https://labuladong.gitee.io/algo/images/backtracking/3.jpg)](https://labuladong.gitee.io/algo/images/backtracking/3.jpg)

**我们定义的 `backtrack` 函数其实就像一个指针，在这棵树上游走，同时要正确维护每个节点的属性，每当走到树的底层叶子节点，其「路径」就是一个全排列**。

再进一步，如何遍历一棵树？这个应该不难吧。回忆一下之前 [学习数据结构的框架思维](https://labuladong.gitee.io/algo/1/2/) 写过，各种搜索问题其实都是树的遍历问题，而多叉树的遍历框架就是这样：

```
void traverse(TreeNode root) {
    for (TreeNode child : root.childern) {
        // 前序位置需要的操作
        traverse(child);
        // 后序位置需要的操作
    }
}
```

> PS：细心的读者肯定会疑问：多叉树 DFS 遍历框架的前序位置和后序位置应该在 for 循环外面，并不应该是在 for 循环里面呀？为什么在回溯算法中跑到 for 循环里面了？
>
> 是的，DFS 算法的前序和后序位置应该在 for 循环外面，不过回溯算法和 DFS 算法略有不同，后文 [图论算法基础](https://labuladong.gitee.io/algo/2/22/50/) 会详细对比，这里可以暂且忽略这个问题。

而所谓的前序遍历和后序遍历，他们只是两个很有用的时间点，我给你画张图你就明白了：

[![](https://labuladong.gitee.io/algo/images/backtracking/4.jpg)](https://labuladong.gitee.io/algo/images/backtracking/4.jpg)

**前序遍历的代码在进入某一个节点之前的那个时间点执行，后序遍历代码在离开某个节点之后的那个时间点执行**。

回想我们刚才说的，「路径」和「选择」是每个节点的属性，函数在树上游走要正确处理节点的属性，那么就要在这两个特殊时间点搞点动作：

[![](https://labuladong.gitee.io/algo/images/backtracking/5.jpg)](https://labuladong.gitee.io/algo/images/backtracking/5.jpg)

现在，你是否理解了回溯算法的这段核心框架？

```
for 选择 in 选择列表:
    # 做选择
    将该选择从选择列表移除
    路径.add(选择)
    backtrack(路径, 选择列表)
    # 撤销选择
    路径.remove(选择)
    将该选择再加入选择列表
```

**我们只要在递归之前做出选择，在递归之后撤销刚才的选择**，就能正确得到每个节点的选择列表和路径。

下面，直接看全排列代码：

```
List<List<Integer>> res = new LinkedList<>();

/* 主函数，输入一组不重复的数字，返回它们的全排列 */
List<List<Integer>> permute(int[] nums) {
    // 记录「路径」
    LinkedList<Integer> track = new LinkedList<>();
    // 「路径」中的元素会被标记为 true，避免重复使用
    boolean[] used = new boolean[nums.length];
    
    backtrack(nums, track, used);
    return res;
}

// 路径：记录在 track 中
// 选择列表：nums 中不存在于 track 的那些元素（used[i] 为 false）
// 结束条件：nums 中的元素全都在 track 中出现
void backtrack(int[] nums, LinkedList<Integer> track, boolean[] used) {
    // 触发结束条件
    if (track.size() == nums.length) {
        res.add(new LinkedList(track));
        return;
    }
    
    for (int i = 0; i < nums.length; i++) {
        // 排除不合法的选择
        if (used[i]) {


            // nums[i] 已经在 track 中，跳过
            continue;
        }
        // 做选择
        track.add(nums[i]);
        used[i] = true;
        // 进入下一层决策树
        backtrack(nums, track, used);
        // 取消选择
        track.removeLast();
        used[i] = false;
    }
}
```

我们这里稍微做了些变通，没有显式记录「选择列表」，而是通过 `used` 数组排除已经存在 `track` 中的元素，从而推导出当前的选择列表：

[![](https://labuladong.gitee.io/algo/images/backtracking/6.jpg)](https://labuladong.gitee.io/algo/images/backtracking/6.jpg)

至此，我们就通过全排列问题详解了回溯算法的底层原理。当然，这个算法解决全排列不是最高效的，你可能看到有的解法连 `used` 数组都不使用，通过交换元素达到目的。但是那种解法稍微难理解一些，这里就不写了，有兴趣可以自行搜索一下。

但是必须说明的是，不管怎么优化，都符合回溯框架，而且时间复杂度都不可能低于 O(N!)，因为穷举整棵决策树是无法避免的。**这也是回溯算法的一个特点，不像动态规划存在重叠子问题可以优化，回溯算法就是纯暴力穷举，复杂度一般都很高**。

明白了全排列问题，就可以直接套回溯算法框架了，下面简单看看 N 皇后问题。

### 二、N 皇后问题

力扣第 51 题「 [N 皇后](https://leetcode.cn/problems/n-queens/)」就是这个经典问题，简单解释一下：给你一个 `N×N` 的棋盘，让你放置 `N` 个皇后，使得它们不能互相攻击。

> PS：皇后可以攻击同一行、同一列、左上左下右上右下四个方向的任意单位。

这个问题本质上跟全排列问题差不多，决策树的每一层表示棋盘上的每一行；每个节点可以做出的选择是，在该行的任意一列放置一个皇后。

因为 C++ 代码对字符串的操作方便一些，所以这道题我用 C++ 来写解法，直接套用回溯算法框架:

```
vector<vector<string>> res;

/* 输入棋盘边长 n，返回所有合法的放置 */
vector<vector<string>> solveNQueens(int n) {
    // vector<string> 代表一个棋盘
    // '.' 表示空，'Q' 表示皇后，初始化空棋盘
    vector<string> board(n, string(n, '.'));
    backtrack(board, 0);
    return res;
}

// 路径：board 中小于 row 的那些行都已经成功放置了皇后
// 选择列表：第 row 行的所有列都是放置皇后的选择
// 结束条件：row 超过 board 的最后一行
void backtrack(vector<string>& board, int row) {
    // 触发结束条件
    if (row == board.size()) {
        res.push_back(board);
        return;
    }
    
    int n = board[row].size();
    for (int col = 0; col < n; col++) {
        // 排除不合法选择
        if (!isValid(board, row, col)) {


            continue;
        }
        // 做选择
        board[row][col] = 'Q';
        // 进入下一行决策
        backtrack(board, row + 1);
        // 撤销选择
        board[row][col] = '.';
    }
}
```

这部分主要代码，其实跟全排列问题差不多，`isValid` 函数的实现也很简单：

```
/* 是否可以在 board[row][col] 放置皇后？ */
bool isValid(vector<string>& board, int row, int col) {
    int n = board.size();
    // 检查列是否有皇后互相冲突
    for (int i = 0; i <= row; i++) {
        if (board[i][col] == 'Q')
            return false;
    }
    // 检查右上方是否有皇后互相冲突
    for (int i = row - 1, j = col + 1; 
            i >= 0 && j < n; i--, j++) {
        if (board[i][j] == 'Q')
            return false;
    }
    // 检查左上方是否有皇后互相冲突
    for (int i = row - 1, j = col - 1;
            i >= 0 && j >= 0; i--, j--) {
        if (board[i][j] == 'Q')
            return false;
    }
    return true;
}
```

> PS：肯定有读者问，按照 N 皇后问题的描述，我们为什么不检查左下角，右下角和下方的格子，只检查了左上角，右上角和上方的格子呢？
>
> 因为皇后是一行一行从上往下放的，所以左下方，右下方和正下方不用检查（还没放皇后）；因为一行只会放一个皇后，所以每行不用检查。也就是最后只用检查上面，左上，右上三个方向。

函数 `backtrack` 依然像个在决策树上游走的指针，通过 `row` 和 `col` 就可以表示函数遍历到的位置，通过 `isValid` 函数可以将不符合条件的情况剪枝：

[![](https://labuladong.gitee.io/algo/images/backtracking/7.jpg)](https://labuladong.gitee.io/algo/images/backtracking/7.jpg)

如果直接给你这么一大段解法代码，可能是懵逼的。但是现在明白了回溯算法的框架套路，还有啥难理解的呢？无非是改改做选择的方式，排除不合法选择的方式而已，只要框架存于心，你面对的只剩下小问题了。

当 `N = 8` 时，就是八皇后问题，数学大佬高斯穷尽一生都没有数清楚八皇后问题到底有几种可能的放置方法，但是我们的算法只需要一秒就可以算出来所有可能的结果。

不过真的不怪高斯，这个问题的复杂度确实非常高，粗略估算一下：

`N` 行棋盘中，第一行有 `N` 个位置可能可以放皇后，第二行有 `N - 1` 个位置，第三行有 `N - 2` 个位置，以此类推，再叠加每次放皇后之前 `isValid` 函数所需的 O(N) 复杂度，所以总的时间复杂度上界是 O(N! \* N)，而且没有什么明显的冗余计算可以优化效率。你可以试试 `N = 10` 的时候，计算就已经很耗时了。

当然，因为有 `isValid` 函数剪枝，并不会真的在每个位置都尝试放皇后，所以实际的执行效率会高一些。但正如后文 [算法时空复杂度分析实用指南](https://labuladong.gitee.io/algo/1/16/) 所说，这个时间复杂度作为上界是没问题的。

**有的时候，如果我们并不想得到所有合法的答案，只想要一个答案，怎么办呢**？比如解数独的算法，找所有解法复杂度太高，只要找到一种解法就可以。

其实特别简单，只要稍微修改一下回溯算法的代码即可：

```
// 函数找到一个答案后就返回 true
bool backtrack(vector<string>& board, int row) {
    // 触发结束条件
    if (row == board.size()) {
        res.push_back(board);
        return true;
    }
    ...
    for (int col = 0; col < n; col++) {
        ...
        board[row][col] = 'Q';

        if (backtrack(board, row + 1))
            return true;
        
        board[row][col] = '.';
    }

    return false;
}
```

这样修改后，只要找到一个答案，for 循环的后续递归穷举都会被阻断。也许你可以在 N 皇后问题的代码框架上，稍加修改，写一个解数独的算法？

### 三、最后总结

回溯算法就是个多叉树的遍历问题，关键就是在前序遍历和后序遍历的位置做一些操作，算法框架如下：

```
def backtrack(...):
    for 选择 in 选择列表:
        做选择
        backtrack(...)
        撤销选择
```

**写 `backtrack` 函数时，需要维护走过的「路径」和当前可以做的「选择列表」，当触发「结束条件」时，将「路径」记入结果集**。

其实想想看，回溯算法和动态规划是不是有点像呢？我们在动态规划系列文章中多次强调，动态规划的三个需要明确的点就是「状态」「选择」和「base case」，是不是就对应着走过的「路径」，当前的「选择列表」和「结束条件」？

动态规划和回溯算法底层都把问题抽象成了树的结构，但这两种算法在思路上是完全不同的。在 [东哥带你刷二叉树（纲领篇）](https://labuladong.gitee.io/algo/2/21/36/) 你将看到动态规划和回溯算法更深层次的区别和联系。

___

**引用本文的题目**

**安装 [我的 Chrome 刷题插件](https://mp.weixin.qq.com/s/X-fE9sR4BLi6T9pn7xP4pg) 点开下列题目可直接查看解题思路：**

| LeetCode | 力扣 |
| --- | --- |
| [112\. Path Sum](https://leetcode.com/problems/path-sum/?show=1) | [112\. 路径总和](https://leetcode.cn/problems/path-sum/?show=1) |
| [113\. Path Sum II](https://leetcode.com/problems/path-sum-ii/?show=1) | [113\. 路径总和 II](https://leetcode.cn/problems/path-sum-ii/?show=1) |
| [140\. Word Break II](https://leetcode.com/problems/word-break-ii/?show=1) | [140\. 单词拆分 II](https://leetcode.cn/problems/word-break-ii/?show=1) |
| [17\. Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/?show=1) | [17\. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/?show=1) |
| [22\. Generate Parentheses](https://leetcode.com/problems/generate-parentheses/?show=1) | [22\. 括号生成](https://leetcode.cn/problems/generate-parentheses/?show=1) |
| [39\. Combination Sum](https://leetcode.com/problems/combination-sum/?show=1) | [39\. 组合总和](https://leetcode.cn/problems/combination-sum/?show=1) |
| [698\. Partition to K Equal Sum Subsets](https://leetcode.com/problems/partition-to-k-equal-sum-subsets/?show=1) | [698\. 划分为k个相等的子集](https://leetcode.cn/problems/partition-to-k-equal-sum-subsets/?show=1) |
| [77\. Combinations](https://leetcode.com/problems/combinations/?show=1) | [77\. 组合](https://leetcode.cn/problems/combinations/?show=1) |
| [78\. Subsets](https://leetcode.com/problems/subsets/?show=1) | [78\. 子集](https://leetcode.cn/problems/subsets/?show=1) |
| \- | [剑指 Offer 34. 二叉树中和为某一值的路径](https://leetcode.cn/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/?show=1) |
| \- | [剑指 Offer II 079. 所有子集](https://leetcode.cn/problems/TVdhkn/?show=1) |
| \- | [剑指 Offer II 080. 含有 k 个元素的组合](https://leetcode.cn/problems/uUsW3B/?show=1) |
| \- | [剑指 Offer II 081. 允许重复选择元素的组合](https://leetcode.cn/problems/Ygoe9J/?show=1) |
| \- | [剑指 Offer II 083. 没有重复元素集合的全排列](https://leetcode.cn/problems/VvJkup/?show=1) |
| \- | [剑指 Offer II 085. 生成匹配的括号](https://leetcode.cn/problems/IDBivT/?show=1) |

___

**引用本文的文章**

-   [BFS 算法解题套路框架](https://labuladong.gitee.io/algo/4/31/110/)
-   [Dijkstra 算法模板及应用](https://labuladong.gitee.io/algo/2/22/56/)
-   [FloodFill算法详解及应用](https://labuladong.github.io/article/fname.html?fname=FloodFill%e7%ae%97%e6%b3%95%e8%af%a6%e8%a7%a3%e5%8f%8a%e5%ba%94%e7%94%a8)
-   [base case 和备忘录的初始值怎么定？](https://labuladong.gitee.io/algo/3/25/72/)
-   [东哥带你刷二叉树（纲领篇）](https://labuladong.gitee.io/algo/2/21/36/)
-   [两种思路解决单词拼接问题](https://labuladong.github.io/article/fname.html?fname=%e5%8d%95%e8%af%8d%e6%8b%bc%e6%8e%a5)
-   [二分搜索怎么用？我和快手面试官进行了深度探讨](https://labuladong.github.io/article/fname.html?fname=%e4%ba%8c%e5%88%86%e5%88%86%e5%89%b2%e5%ad%90%e6%95%b0%e7%bb%84)
-   [分治算法详解：运算优先级](https://labuladong.gitee.io/algo/4/33/122/)
-   [前缀树算法模板秒杀五道算法题](https://labuladong.gitee.io/algo/2/23/61/)
-   [动态规划和回溯算法到底谁是谁爹？](https://labuladong.gitee.io/algo/3/27/84/)
-   [回溯算法最佳实践：括号生成](https://labuladong.gitee.io/algo/4/31/109/)
-   [回溯算法最佳实践：解数独](https://labuladong.gitee.io/algo/4/31/108/)
-   [回溯算法秒杀所有排列/组合/子集问题](https://labuladong.gitee.io/algo/4/31/106/)
-   [图论基础及遍历算法](https://labuladong.gitee.io/algo/2/22/50/)
-   [学习算法和刷题的框架思维](https://labuladong.gitee.io/algo/1/2/)
-   [我的刷题心得](https://labuladong.gitee.io/algo/1/3/)
-   [环检测及拓扑排序算法](https://labuladong.gitee.io/algo/2/22/51/)
-   [算法学习和心流体验](https://labuladong.github.io/article/fname.html?fname=%e5%bf%83%e6%b5%81)
-   [算法笔试「骗分」套路](https://labuladong.gitee.io/algo/1/17/)
-   [经典动态规划：戳气球](https://labuladong.gitee.io/algo/3/28/92/)
-   [经典回溯算法：集合划分问题](https://labuladong.gitee.io/algo/4/31/105/)

___

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

**《labuladong 的算法小抄》已经出版，关注公众号查看详情；后台回复关键词「**进群**」可加入算法群；回复「**全家桶**」可下载配套 PDF 和刷题全家桶**：

[![](https://labuladong.gitee.io/algo/images/souyisou2.png)](https://labuladong.gitee.io/algo/images/souyisou2.png)