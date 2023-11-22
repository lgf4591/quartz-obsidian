---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:32:10
tags: [learn/program/algorithm/labuladong/common]
title: Linux 管道和重定向的坑  labuladong的算法小抄
---
# Linux 管道和重定向的坑 Labuladong的算法小抄
[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

**———–**

我很喜欢 Linux 系统，尤其是 Linux 的一些设计很漂亮，比如可以将一些复杂的问题分解成若干小问题，通过管道符和重定向机制灵活地用现成的工具解决，写成 shell 脚本就很高效。

前文写过好几篇 Linux 相关的文章：

-   [Linux 文件系统都是什么鬼](https://labuladong.gitee.io/algo/5/35/)
-   [Linux shell 必知必会的技巧](https://labuladong.gitee.io/algo/5/37/)
-   [Linux 进程/管道/重定向/文件描述符](https://labuladong.gitee.io/algo/5/36/)

本文就分享一下我在实践中使用重定向和管道符遇到的一些坑，搞明白一些底层原理，写脚本的效率能提升不少。

## \> 和 » 重定向符的坑

先说第一个问题，执行如下命令会发生什么？

```
$ cat file.txt > file.txt
```

读取再写入同一个文件，感觉什么也不会发生对吧？

**实际上，上述命令运行的结果是清空 `file.txt` 文件中的内容**。

> PS：有的 Linux 发行版可能会直接报错，可以执行 `cat < file.txt > file.txt` 绕开这个检测。

前文 [Linux 进程和文件描述符](https://labuladong.gitee.io/algo/5/36/) 说过，程序本身没有必要关心自己的标准输入/输出指向哪里，是 shell 通过管道符和重定向符号修改了程序的标准输入/输出的位置。

所以执行 `cat file.txt > file.txt` 这个命令时，shell 会先打开 `file.txt`，由于重定向符号是 `>`，所以文件中的内容会被清空，然后 shell 将 `cat` 命令的标准输出设置为 `file.txt`，这时候 `cat` 命令才开始执行。

也就是如下过程：

1、shell 打开 `file.txt` 并清空其内容。

2、shell 将 `cat` 命令的标准输出指向 `file.txt` 文件。

3、shell 执行 `cat` 命令，读了一个空文件。

4、`cat` 命令将空字符串写入标准输出（`file.txt` 文件）。

所以，最后的结果就是 `file.txt` 变成了空文件。

我们知道，`>` 会清空目标文件，`>>` 会在目标文件尾部追加内容，**那么如果将重定向符 `>` 改成 `>>` 会怎样呢**？

```
$ echo hello world > file.txt 
$ cat file.txt >> file.txt 
```

`file.txt` 中首先被写入一行内容，执行 `cat file.txt >> file.txt` 后预期的结果应该是两行内容。

但是很遗憾，运行结果并不符合预期，而是会死循环不断向 `file.txt` 中写入 hello world，文件很快就会变得很大，只能用 Control+C 停止命令。

这就有意思了，为什么会死循环呢？其实稍加分析就可以想到原因：

首先要回忆 `cat` 命令的行为，如果只执行 `cat` 命令，就会从命令行读取键盘输入的内容，每次按下回车，`cat` 命令就会回显输入，也就是说，`cat` 命令是逐行读取数据然后输出数据的。

那么，`cat file.txt >> file.txt` 命令的执行过程如下：

1、打开 `file.txt`，准备在文件尾部追加内容。

2、将 `cat` 命令的标准输出指向 `file.txt` 文件。

3、`cat` 命令读取 `file.txt` 中的一行内容并写入标准输出（追加到 `file.txt` 文件中）。

4、由于刚写入了一行数据，`cat` 命令发现 `file.txt` 中还有可以读取的内容，就会重复步骤 3。

**以上过程，就好比一边遍历列表，一遍往列表里追加元素一样，永远遍历不完，所以导致我们的命令死循环**。

## \> 重定向符和 | 管道符配合

我们经常会遇到这样的需求：截取文件的前 XX 行，其余的都删除。

在 Linux 中，`head` 命令可以完成截取文件前几行的功能：

```

$ cat file.txt 
1
2
3
4
5
$ head -n 2 file.txt 
1
2
$ cat file.txt | head -n 2 
1
2
```

如果我们想保留文件的前 2 行，其他的都删除，可能会用如下命令：

```
$ head -n 2 file.txt > file.txt
```

但是这就犯了前文说的错误，最后 `file.txt` 会被清空，不能实现我们的需求。

那我们是这样写命令是否可以避坑呢：

```
$ cat file.txt | head -n 2 > file.txt
```

**结论是不行，因为管道符连接的多个命令是并行执行的**。

前文 [Linux 进程和文件描述符](https://labuladong.gitee.io/algo/5/36/) 也说过管道符的实现原理，本质上就是将两个命令的标准输入和输出连接起来，让前一个命令的标准输出作为下一个命令的标准输入。

但是，如果你认为这样写命令可以得到预期的结果，那可能是因为你认为管道符连接的命令是串行执行的，这是一个常见的错误。

你可能以为，shell 会先执行 `cat file.txt` 命令，正常读取 `file.txt` 中的所有内容，然后把这些内容通过管道传递给 `head -n 2 > file.txt` 命令。

虽然这时候 `file.txt` 中的内容会被清空，但是 `head` 并没有从文件中读取数据，而是从管道读取数据，所以应该可以向 `file.txt` 正确写入两行数据。

但实际上，上述理解是错误的，shell 会并行执行管道符连接的命令，比如说执行如下命令：

shell 会同时启动两个 `sleep` 进程，所以执行结果是睡眠 5 秒，而不是 10 秒。

这是有点违背直觉的，比如这种常见的命令：

```
$ cat filename | grep 'pattern'
```

直觉好像是先执行 `cat` 命令一次性读取了 `filename` 中所有的内容，然后传递给 `grep` 命令进行搜索。

但实际上是 `cat` 和 `grep` 命令是同时执行的，之所以能得到预期的结果，是因为 `grep 'pattern'` 会阻塞等待标准输入，而 `cat` 通过 Linux 管道向 `grep` 的标准输入写入数据。

执行下面这个命令能直观感受到 `cat` 和 `grep` 是在同时执行的，`grep` 在实时处理我们用键盘输入的数据：

说了这么多，再回顾一开始的问题：

```
$ cat file.txt | head -n 2 > file.txt
```

**`cat` 命令和 `head` 会并行执行，谁先谁后不确定，执行结果也就不确定**。

如果 `head` 命令先于 `cat` 执行，那么 `file.txt` 就会被先清空，`cat` 也就读取不到任何内容；反之，如果 `cat` 先把文件的内容读取出来，那么可以得到预期的结果。

不过，通过我的实验（将这种并发情况重复 1w 次）发现，`file.txt` 被清空这种错误情况出现的概率远大于预期结果出现的概率，这个暂时还不清楚是为什么，应该和 Linux 内核实现进程和管道的逻辑有关。

## 解决方案

说了这么多管道符和重定向符的特点，如何才能避免这个文件被清空的坑呢？

**最靠谱的办法就是不要同时对同一个文件进行读写，而是通过临时文件的方式做一个中转**。

比如说只保留 `file.txt` 文件中的头两行，可以这样写代码：

```
# 先把数据写入临时文件，然后覆盖原始文件
$ cat file.txt | head -n 2 > temp.txt && mv temp.txt file.txt
```

**这是最简单，最可靠，万无一失的方法**。

你如果嫌这段命令太长，也可以通过 `apt/brew/yum` 等包管理工具安装 `moreutils` 包，就会多出一个 `sponge` 命令，像这样使用：

```
# 先把数据传给 sponge，然后由 sponge 写入原始文件
$ cat file.txt | head -n 2 | sponge file.txt
```

`sponge` 这个单词的意思是海绵，挺形象的，它会先把输入的数据「吸收」起来，最后再写入 `file.txt`，核心思路和我们使用临时文件时类似的，这个「海绵」就好比一个临时文件，就可以避免同时打开同一个文件进行读写的问题。

以上就是重定向和管道符的一些坑，希望能帮到你。

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

**《labuladong 的算法小抄》已经出版，关注公众号查看详情；后台回复关键词「**进群**」可加入算法群；回复「**全家桶**」可下载配套 PDF 和刷题全家桶**：

[![](https://labuladong.gitee.io/algo/images/souyisou2.png)](https://labuladong.gitee.io/algo/images/souyisou2.png)