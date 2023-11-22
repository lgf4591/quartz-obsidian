---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:32:36
tags: [learn/program/algorithm/labuladong/common]
title: 我用四个命令概括了 Git 的所有套路  labuladong的算法小抄
---
# 我用四个命令概括了 Git 的所有套路 Labuladong的算法小抄
[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

**———–**

我先开一会儿吐槽大会，Git 这东西我用了两年，根本尼玛用不明白。

我搞不明白的一个重要原因就是，命令的功能太杂，有时候一个需求可以用好几种命令解决，而且有的命令还 tm 有别名。这导致什么问题呢，我在网上找到的答案五花八门，竟然都能达成目的，难以找到规律，毫无套路可言。对于我这种不喜欢动脑子，只喜欢玩套路的人来说，简直不能接受。

以前我用 Git，就知道 `add .`，然后 `commit -m`，最后 `push origin master` 一套带走，或者就是把 Git 作为下载器，去 `clone` 别人的项目。但是在工作中呢，和别人一起开发代码，就需要处理一些复杂情况，比如解决冲突，比如手残恢复，等等等实用场景，这些我在后文都会列举。

对于工具的学习，我认为应该多做减法，只捡最有用的学，太复杂的技巧不学也罢，应该把时间投入更有价值的事情中。

**所以本文不是一个大而全 Git 命令的使用手册，而是根据实际工作中最常见问题，提供小而美的解决方案，仅仅涉及四个命令：`add`，`commit`，`reset`，`checkout`**。

## 一、前言

首先，在进入 Git 的各种操作之前，**一定要明白 git 的三个「分区」是什么，否则的话你一定没办法真正理解 Git 的原理**。

Git 的三个分区分别是：`working directory`，`stage/index area`，`commit history`。

[![](https://labuladong.gitee.io/algo/images/git/trees.png)](https://labuladong.gitee.io/algo/images/git/trees.png)

**`working directory` 是「工作目录」，也就是我们肉眼能够看到的文件**，后文我们称其为 `work dir` 区。

当我们在 `work dir` 中**执行 `git add` 相关命令后，就会把 `work dir` 中的修改添加到「暂存区」`stage area`（或者叫 `index area`）中去**，后文我们称暂存区为 `stage` 区。

当 `stage` 中存在修改时，我们**使用 `git commit` 相关命令之后，就会把 `stage` 中的修改保存到「提交历史」 `commit history` 中**，也就是 `HEAD` 指针指向的位置。后文我们称「提交历史」为 `history` 区。

关于 `commit history` 我们多说几句，任何修改只要进入 `commit history`，基本可以认为永远不会丢失了。每个 `commit` 都有一个唯一的 Hash 值，我们经常说的 `HEAD` 或者 `master` 分支，都可以理解为一个指向某个 `commit` 的指针。

`work dir` 和 `stage` 区域的状态，可以通过命令 `git status` 来查看，`history` 区域的提交历史可以通过 `git log` 命令来查看。

好的，如果上面的内容你都能够理解，那么本文就完全围绕这三个概念展开，下面就是一个「状态转移图」：

[![](https://labuladong.gitee.io/algo/images/git/1.jpeg)](https://labuladong.gitee.io/algo/images/git/1.jpeg)

## 二、本地 Git 极简教程

**需求一，如何把 `work dir` 中的修改加入 `stage`**。

[![](https://labuladong.gitee.io/algo/images/git/2.jpeg)](https://labuladong.gitee.io/algo/images/git/2.jpeg)

这个是最简单，使用 **`git add`** 相关的命令就行了。顺便一提，`add` 有个别名叫做 `stage`，也就是说你可能见到 `git stage` 相关的命令，这个命令和 `git add` 命令是完全一样的。

使用建议：可以比较随意随意使用。

理由：不会改变任或撤销任何已作出的修改，而且还会将 `work dir` 中未追踪的修改（Untracked file）添加到暂存区 `stage` 中进行追踪。

**需求二，如何把 `stage` 中的修改还原到 `work dir` 中**。

[![](https://labuladong.gitee.io/algo/images/git/3.jpeg)](https://labuladong.gitee.io/algo/images/git/3.jpeg)

这个需求很常见，也很重要，比如我先将当前 `work dir` 中的修改添加到 `stage` 中，然后又对 `work dir` 中的文件进行了修改，但是又后悔了，如何把 `work dir` 中的全部或部分文件还原成 `stage` 中的样子呢？

来个实际场景，我先新建两个文件，然后把他们都加到 `stage`：

```
$ touch a.txt b.txt
$ git add .
$ git status
On branch master
Changes to be committed:
new file:   a.txt
new file:   b.txt
```

然后我又修改了 `a.txt` 文件：

```
$ echo hello world >> a.txt
$ git status
On branch master
Changes to be committed:
new file:   a.txt
new file:   b.txt

Changes not staged for commit:
modified:   a.txt
```

现在，我后悔了，我认为不应该修改 `a.txt`，我想把它还原成 `stage` 中的空文件，怎么办？

答案是，使用 **`checkout`** 命令：

```
$ git checkout a.txt
Updated 1 path from the index

$ git status
On branch master
Changes to be committed:
new file:   a.txt
new file:   b.txt
```

看到了么，输出显示从 `index` 区（也就是 `stage` 区）更新了一个文件，也就是把 `work dir` 中 `a.txt` 文件还原成了 `stage` 中的状态（一个空文件）。

当然，如果 `work dir` 中被修改的文件很多，可以使用通配符全部恢复成 `stage`：

有一点需要指出的是，`checkout` 命令只会把被「修改」的文件恢复成 `stage` 的状态，如果 `work dir` 中新增了新文件，你使用 `git checkout .` 是不会删除新文件的。

使用建议：小心使用。

理由：在 `work dir` 做出的「修改」会被 `stage` 覆盖，无法恢复。所以使用该命令你应该确定 `work dir` 中的修改可以抛弃。

**需求三，将 `stage` 区的文件添加到 `history` 区**。

[![](https://labuladong.gitee.io/algo/images/git/4.jpeg)](https://labuladong.gitee.io/algo/images/git/4.jpeg)

很简单，就是 **`git commit`** 相关的命令，一般我们就是这样用的：

再简单提一些常见场景， 比如说 `commit` 完之后，突然发现一些错别字需要修改，又不想为改几个错别字而新开一个 `commit` 到 `history` 区，那么就可以使用下面这个命令：

这样就是把错别字的修改和之前的那个 `commit` 中的修改合并，作为一个 `commit` 提交到 `history` 区。

使用建议：可以比较随意随意使用。

理由：不会改变任或撤销任何已作出的修改，而且还会将 `stage` 区的修改加入 `history` 区并分配一个 Hash 值。只要不乱动本地的 `.git` 文件夹，进入 `history` 的修改就永远不会丢失。

**需求四，将 `history` 区的文件还原到 `stage` 区**。

[![](https://labuladong.gitee.io/algo/images/git/5.jpeg)](https://labuladong.gitee.io/algo/images/git/5.jpeg)

这个需求很常见，比如说我用了一个 `git add .` 一股脑把所有修改加入 `stage`，但是突然想起来文件 `a.txt` 中的代码我还没写完，不应该把它 `commit` 到 `history` 区，所以我得把它从 `stage` 中撤销，等后面我写完了再提交。

```
$ echo aaa >> a.txt; echo bbb >> b.txt;
$ git add .
$ git status
On branch master
Changes to be committed:
modified:   a.txt
modified:   b.txt
```

如何把 `a.txt` 从 `stage` 区还原出来呢？可以使用 **`git reset`** 命令：

```
$ git reset a.txt

$ git status
On branch master
Changes to be committed:
modified:   b.txt

Changes not staged for commit:
modified:   a.txt
```

你看，这样就可以把 `a.txt` 文件从 `stage` 区移出，这时候进行 `git commit` 相关的操作就不会把这个文件一起提交到 `history` 区了。

上面的这个命令是一个简写，实际上 `reset` 命令的完整写法如下：

```
$ git reset --mixed HEAD a.txt
```

其中，`mixed` 是一个模式（mode）参数，如果 `reset` 省略这个选项的话默认是 `mixed` 模式；`HEAD` 指定了一个历史提交的 hash 值；`a.txt` 指定了一个或者多个文件。

**该命令的自然语言描述是：不改变 `work dir` 中的任何数据，将 `stage` 区域中的 `a.txt` 文件还原成 `HEAD` 指向的 `commit history` 中的样子**。就相当于把对 `a.txt` 的修改从 `stage` 区撤销，但依然保存在 `work dir` 中，变为 `unstage` 的状态。

使用建议：小心使用。

理由：不会改变 `work dir` 中的数据，会改变 `stage` 区的数据，所以应确保 `stage` 中被改动数据是可以抛弃的。

**需求五，将 `work dir` 的修改提交到 `history` 区**。

[![](https://labuladong.gitee.io/algo/images/git/6.jpeg)](https://labuladong.gitee.io/algo/images/git/6.jpeg)

这个需求很简单啦，先 `git add` 然后 `git commit` 就行了，或者一个快捷方法是使用命令 `git commit -a`。

使用建议：可以比较随意随意使用。

理由：commit 之后的数据不会丢失。

**需求六，将 `history` 区的历史提交还原到 `work dir` 中**。

[![](https://labuladong.gitee.io/algo/images/git/7.jpeg)](https://labuladong.gitee.io/algo/images/git/7.jpeg)

这个场景，我说一个极端一点的例子：比如我从 GitHub 上 `clone` 了一个项目，然后乱改了一通代码，结果发现我写的代码根本跑不通，于是后悔了，干脆不改了，我想恢复成最初的模样，怎么办？

依然是使用 `checkout` 命令，但是和之前的使用方式有一些不同：

```
$ git checkout HEAD .
Updated 12 paths from d480c4f
```

这样，`work dir` 和 `stage` 中所有的「修改」都会被撤销，恢复成 `HEAD` 指向的那个 `history commit`。

注意，类似之前通过 `stage` 恢复 `work dir` 的 `checkout` 命令，这里撤销的也只是修改，新增的文件不会被撤销。

当然，只要找到任意一个 `commit` 的 HASH 值，`checkout` 命令可就以将文件恢复成任一个 `history commit` 中的样子：

```
$ git checkout 2bdf04a some_test.go
Updated 1 path from 2bdf04a
# 前文的用法显示 update from index
```

比如，我改了某个测试文件，结果发现测试跑不过了，所以就把该文件恢复到了它能跑过的那个历史版本……

使用建议：谨慎使用。

理由：这个操作会将指定文件在 `work dir` 的数据恢复成指定 `commit` 的样子，且会删除该文件在 `stage` 中的数据，都无法恢复，所以应该慎重使用。

## 三、其他技巧

**需求一，合并多个 `commit`**。

比如说我本地从 `17bd20c` 到 `HEAD` 有多个 `commit`，但我希望把他们合并成一个 `commit` 推到远程仓库，这时候就可以使用 `reset` 命令：

```
$ git reset 17bd20c
$ git add .
$ git commit -m 'balabala'
```

回顾一下刚才说的 `reset` 命令的作用，相当于把 HEAD 移到了 `17bd20c` 这个 `commit`，而且不会修改 `work dir` 中的数据，所以只要 `add` 再 `commit`，就相当于把中间的多个 `commit` 合并到一个了。

**需求二，由于 `HEAD` 指针的回退，导致有的 `commit` 在 `git log` 命令中无法看到，怎么得到它们的 Hash 值呢**？

再重复一遍，只要你不乱动本地的 `.git` 文件夹，任何修改只要提交到 `commit history` 中，都永远不会丢失，看不到某些 `commit` 只是因为它们不是我们当前 `HEAD` 位置的「历史」提交，我们可以使用如下命令查看操作记录：

比如 `reset`，`checkout` 等等关键操作都会在这里留下记录，所有 `commit` 的 Hash 值都能在这里找到，所以如果你发现有哪个 `commit` 突然找不到了，一定都可以在这里找到。

**需求三，怎么解决冲突**？

记住，Git 虽然高大上，但也不要迷恋，一定要懂得借助先进的工具。

比较流行的代码编辑器或者 IDE 都会集成方便的可视化 Git 工具，至于解决冲突，可视化的表现方式不是比你在命令行里 `git diff` 看半天要清晰明了得多？只需要点点点就行了。

所以说，只要明白本文讲的这些基本操作，够你用的了，平时能用图形化工具就多用图形化工具，毕竟工具都是为人服务的。

后文 [Git/SQL/正则表达式的在线练习平台](https://labuladong.gitee.io/algo/5/42/) 介绍了一个优秀的 Git 在线练习平台，值得一刷。

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

**《labuladong 的算法小抄》已经出版，关注公众号查看详情；后台回复关键词「**进群**」可加入算法群；回复「**全家桶**」可下载配套 PDF 和刷题全家桶**：

[![](https://labuladong.gitee.io/algo/images/souyisou2.png)](https://labuladong.gitee.io/algo/images/souyisou2.png)