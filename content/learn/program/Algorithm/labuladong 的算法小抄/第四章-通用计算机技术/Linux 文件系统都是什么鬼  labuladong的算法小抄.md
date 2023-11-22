---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:32:01
tags: [learn/program/algorithm/labuladong/common]
title: Linux 文件系统都是什么鬼  labuladong的算法小抄
---
# Linux 文件系统都是什么鬼 Labuladong的算法小抄
[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

**———–**

Linux 系统大家一定不陌生，学习工作中肯定会和 Linux 打交道。不过谈及 Linux，给人的第一印象可能就是黑乎乎的命令行，乱七八糟的文件夹，甚至安装软件都不知道是怎么安装上去的。

其实这些问题都是用惯了 Windows 的后遗症，多用用 Linux 就会发现这玩意还是挺好用的，关键还能 DIY，非常装逼：

[![](https://labuladong.gitee.io/algo/images/linux-fs/desktop.png)](https://labuladong.gitee.io/algo/images/linux-fs/desktop.png)

言归正传，本文介绍一下 Linux 的文件系统（File System），简单介绍一下这些文件夹都是干什么的。

[![](https://labuladong.gitee.io/algo/images/linux-fs/linux-filesystem.png)](https://labuladong.gitee.io/algo/images/linux-fs/linux-filesystem.png)

上图是 Linux 文件系统的一个整体结构，无论是什么版本的 Linux 发行版，根目录 `/` 基本上就是这些文件。不要害怕这么多文件夹，大部分都不需要你操心，只要大概了解它们是干啥的就行了。

## /bin 和 /sbin

`bin` 是 `Binary` 的缩写，存放着可执行文件或可执行文件的链接（类似快捷方式），如下图：

[![](https://labuladong.gitee.io/algo/images/linux-fs/bin.png)](https://labuladong.gitee.io/algo/images/linux-fs/bin.png)

你可以看到 `cp`, `chmod`, `cat` 等常用命令都在这里。注意不要被图标上的 TXT 字样误导了，Linux 不按后缀识别文件类型，`/bin` 目录中的文件都是可执行的二进制文件，而不是文本文件。

与 `/bin` 类似的是 `/sbin` 目录，System Binary 的缩写，这里存放的命令可以对系统配置进行操作。普通用户可能可以使用这里的命令查看某些系统状态，但是如果想更改配置，就需要 `sudo` 授权或者切换成超级用户。

[![](https://labuladong.gitee.io/algo/images/linux-fs/sbin.png)](https://labuladong.gitee.io/algo/images/linux-fs/sbin.png)

可以看到一些熟悉的命令，比如 `ifconfig`, `iptables`。普通用户可以使用 `ifconfig` 查看网卡状态，但是想配置网卡信息，就需要授权了。

## /boot

[![](https://labuladong.gitee.io/algo/images/linux-fs/boot.png)](https://labuladong.gitee.io/algo/images/linux-fs/boot.png)

这里是系统启动需要的文件，你可以看到 `grub` 文件夹，它是常见的开机引导程序。我们不应该乱动这里的文件。

## /dev

`dev` 是 `device` 的缩写，这里存放这所有的设备文件。在 Linux 中，所有东西都是以文件的形式存在的，包括硬件设备。

比如说，`sda`, `sdb` 就是我电脑上的两块硬盘，后面的数字是硬盘分区：

[![](https://labuladong.gitee.io/algo/images/linux-fs/dev.png)](https://labuladong.gitee.io/algo/images/linux-fs/dev.png)

鼠标、键盘等等设备也都可以在这里找到。

## /etc

这个目录经常使用，存放很多程序的配置信息，比如包管理工具 apt：

[![](https://labuladong.gitee.io/algo/images/linux-fs/etc.png)](https://labuladong.gitee.io/algo/images/linux-fs/etc.png)

在 `/etc/apt` 中就存放着对应的配置，比如说镜像列表（我配置的阿里云镜像）：

[![](https://labuladong.gitee.io/algo/images/linux-fs/apt.png)](https://labuladong.gitee.io/algo/images/linux-fs/apt.png)

如果你要修改一些系统程序的配置，十有八九要到 `etc` 目录下寻找。

## /lib

`lib` 是 Library 的缩写，类似于 Windows 系统中存放 `dll` 文件的库，包含 bin 和 sbin 中可执行文件的依赖。

也可能出现 `lib32` 或 `lib64` 这样的目录，和 `lib` 差不多，只是操作系统位数不同而已。

## /media

这里会有一个以你用户名命名的文件夹，里面是自动挂载的设备，比如 U 盘，移动硬盘，网络设备等。

比如说我在电脑上插入一个 U 盘，系统会把 U 盘自动给我挂载到 `/media/fdl` 这个文件夹里（我的用户名是 fdl），如果我要访问 U 盘的内容，就可以在那里找到。

## /mnt

这也是和设备挂载相关的一个文件夹，一般是空文件夹。`media` 文件夹是系统自动挂载设备的地方，这里是你手动挂载设备的地方。

比如说，刚才我们在 `dev` 中看到了一大堆设备，你想打开某些设备看看里面的内容，就可以通过命令把设备挂载到 `mnt` 目录进行操作。

不过一般来说，现在的操作系统已经很聪明了，像挂载设备的操作几乎都不用你手动做，都被自动挂载到 `media` 目录了。

## /opt

`opt` 是 Option 的缩写，这个文件夹的使用比较随意，一般来说我们自己在浏览器上下载的软件，安装在这里比较好。当然，包管理工具下载的软件也可能被存放在这里。

[![](https://labuladong.gitee.io/algo/images/linux-fs/opt.png)](https://labuladong.gitee.io/algo/images/linux-fs/opt.png)

比如我在这里存放了 Chrome 浏览器（google），网易云音乐（netease），CLion IDE 等等软件的文件夹。

## /proc

`proc` 是 `process` 的缩写，这里存放的是全部正在运行程序的状态信息。

[![](https://labuladong.gitee.io/algo/images/linux-fs/proc.png)](https://labuladong.gitee.io/algo/images/linux-fs/proc.png)

你会发现 `/proc` 里面有一大堆数字命名的文件夹，这个数字其实是 Process ID（PID），文件夹里又有很多文件。

前面说过，Linux 中一切都以文件形式储存，类似 `/dev`，这里的文件也不是真正的文件，而是程序和内核交流的一些信息。比如说我们可以查看当前操作系统的版本，或者查看 CPU 的状态：

[![](https://labuladong.gitee.io/algo/images/linux-fs/cpu.png)](https://labuladong.gitee.io/algo/images/linux-fs/cpu.png)

如果你需要调试应用程序，`proc` 目录中的信息也许会帮上忙。

## /root

这是 root 用户的家目录，普通用户需要授权才能访问。

## /run 和 /sys

用来存储某些程序的运行时信息和系统需要的一些信息。比如说这个文件：

```
sudo vim /sys/devices/pci0000:00/0000:00:02.0/drm/card0/card0-eDP-1/intel_backlight/brightness
```

里面存储着一个数字，是你的显卡亮度，你修改这个数字就可以修改屏幕亮度。

需要注意的是，这两个位置的数据都存储在内存中，所以一旦重启，`/run` 和 `/sys` 目录的信息就会丢失，所以不要试图在这里存放任何文件。

## /srv

`srv` 是 `service` 的缩写，主要用来存放服务数据。

对于桌面版 Linux 系统，这个文件夹一般是空的，但是对于 Linux 服务器，Web 服务或者 FTP 文件服务的资源可以存放在这里。

## /tmp

`tmp` 是 `temporary` 的缩写，存储一些程序的临时文件。

[![](https://labuladong.gitee.io/algo/images/linux-fs/tmp.png)](https://labuladong.gitee.io/algo/images/linux-fs/tmp.png)

临时文件可能起到很重要的作用。比如经常听说某同学的 Word 文档崩溃了，好不容易写的东西全没了，Linux 的很多文本编辑器都会在 `/tmp` 放一份当前文本的 copy 作为临时文件，如果你的编辑器意外崩溃，还有机会在 `/tmp` 找一找临时文件抢救一下。

比如上图的 `VSCode Crashes` 应该就是 VScode 编辑器存放临时文件的地方。

当然，`tmp` 文件夹在系统重启之后会自动被清空，如果没有被清空，说明系统删除某些文件失败，也许需要你手动删除一下。

## /usr

`usr` 是 Universal System Resource 的缩写，这里存放的是一些**非系统必须的资源**，比如用户安装的应用程序。

[![](https://labuladong.gitee.io/algo/images/linux-fs/usr.png)](https://labuladong.gitee.io/algo/images/linux-fs/usr.png)

`/usr` 和 `/usr/local` 目录中又含有 `bin` 和 `sbin` 目录，也是存放可执行文件（命令），但和根目录的 `bin` 和 `sbin` 不同的是，这里大都是用户使用的工具，而非系统必须使用的。

比如说 `/usr/bin` 中含有我通过包管理工具安装的应用程序 Chrome 浏览器和 goldendict 字典的可执行文件：

[![](https://labuladong.gitee.io/algo/images/linux-fs/usrbin.png)](https://labuladong.gitee.io/algo/images/linux-fs/usrbin.png)

值得一提的是，如果使用 Linux 桌面版，有时候在桌面找不到应用程序的快捷方式，就需要在 `/usr/share/applications` 中手动配置桌面图标文件：

[![](https://labuladong.gitee.io/algo/images/linux-fs/application.png)](https://labuladong.gitee.io/algo/images/linux-fs/application.png)

## /var

`var` 是 `variable` 的缩写，这个名字是历史遗留的，现在该目录最主要的作用是存储日志（log）信息，比如说程序崩溃，系统异常等等信息都会记录在这里。

这是我的 `/var/log` 目录：

[![](https://labuladong.gitee.io/algo/images/linux-fs/log.png)](https://labuladong.gitee.io/algo/images/linux-fs/log.png)

日志文件不会自动删除，也就是说随着系统使用时间的增长，你的 `var` 目录占用的磁盘空间会越来越大，也许需要适时清理一下。

## /home

最后说 `home` 目录，这是普通用户的家目录。在桌面版的 Linux 系统中，用户的家目录会有下载、视频、音乐、桌面等文件夹，这些没啥可说的，我们说一些隐藏的比较重要的文件夹（Linux 中名称以 `.` 开头就是隐藏文件）。

这是我的家目录的部分文件：

[![](https://labuladong.gitee.io/algo/images/linux-fs/home.png)](https://labuladong.gitee.io/algo/images/linux-fs/home.png)

其中 `.cache` 文件夹存储应用缓存数据，`.config` 文件夹存储了一部分应用程序的配置，比如说我的 Chrome 浏览器配置就是那里面。但是还有一部分应用程序并不把配置储存在 `.config` 文件夹，而是自己创建一个隐藏文件夹，存放自己的配置文件等等信息，比如你可以看到 Intellij 的配置文件就不在 `.config` 中。

最后说 `.local` 文件夹，有点像 `/usr/local`，里面也有 `bin` 文件夹，也是存放可执行文件的。比如说我的 python pip 以及 pip 安装的一些工具，都存放在 `～/.local/bin` 目录中。**但是，存在这里的文件，只有该用户才能使用**。

这就是为什么，有时候普通用户可以使用的命令，用 sudo 或者超级用户却被告知找不到该命令。因为有的命令是特定用户家目录里的，被添加到了该用户的 `PATH` 环境变量里，他可以直接用。你超级用户想用当然可以，但是得写全绝对路径才行。

## 最后总结

如果修改系统配置，就去 `/etc` 找，如果修改用户的应用程序配置，就在用户家目录的隐藏文件里找。

你在命令行里可以直接输入使用的命令，其可执行文件一般就在以下几个位置：

```
/bin    
/sbin
/usr/bin
/usr/sbin
/usr/local/bin
/usr/local/sbin
/home/USER/.local/bin
/home/USER/.local/sbin
```

如果你写了一个脚本/程序，想在任何时候都能直接调用，可以把这个脚本/程序添加到上述目录中。

如果某个程序崩溃了，可以到 `/val/log` 中尝试寻找出错信息，到 `/tmp` 中寻找残留的临时文件。

设备文件在 `/dev` 目录，但是一般来说系统会自动帮你挂载诸如 U 盘之类的设备，可以到 `/media` 文件夹访问设备内容。

___

**引用本文的文章**

-   [Linux 管道和重定向的坑](https://labuladong.gitee.io/algo/5/38/)

___

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

**《labuladong 的算法小抄》已经出版，关注公众号查看详情；后台回复关键词「**进群**」可加入算法群；回复「**全家桶**」可下载配套 PDF 和刷题全家桶**：

[![](https://labuladong.gitee.io/algo/images/souyisou2.png)](https://labuladong.gitee.io/algo/images/souyisou2.png)