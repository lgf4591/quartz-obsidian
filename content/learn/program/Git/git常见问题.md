---
aliases: [Git常见问题 ]
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:53:57
tags: [learn/program/git, learn/program/common]
title: Git常见问题 
---
# Git常见问题
**an editor opened by 'git commit'. Please make sure all processes**

可能是上次提交时网络异常，造成`index.lock`文件没有删除，所以手动 `.git/index.lock`文件即可

**ssh: connect to host github.com port 22: Connection refused**

向github推送代码时显示上面的错误，首先创建文件 `~/.ssh/config`，然后添加以下内容

```
Host github.com  
User 你的github邮箱
Hostname ssh.github.com  
PreferredAuthentications publickey  
IdentityFile ~/.ssh/id_rsa  
Port 443
```

**Failed to connect to github.com port 443**

首先访问[ipaddress.com (opens new window)](https://ipaddress.com/website/github.com) 获取 github 的ip地址

> 这个网站有可能你访问不了，你要自己想想办法了

![image-20220730142548534](https://doc.houdunren.com/assets/img/image-20220730142548534.37f77650.png)

然后在修改 hosts 文件，添加记录

```
140.82.113.3 github.com
```