

只需三步，[Quartz](https://github.com/jackyzha0/quartz) 就能快速发布你的 [Obsidian](https://zigholding.github.io/quartz/%E7%9F%A5%E8%AF%86%E4%BD%93%E7%B3%BB/Obsidian/Obsidian) 笔记。

一、[安装 NodeJs](https://zigholding.github.io/quartz/%E7%9F%A5%E8%AF%86%E4%BD%93%E7%B3%BB/Obsidian/%E5%8F%91%E5%B8%83Obsidian/%E5%AE%89%E8%A3%85-NodeJs)，选择 `v18.14` 以上版本。

二、下载 `quartz` 项目，执行以下命令：

```
git clone https://github.com/jackyzha0/quartz.git
cd quartz
npm i
npx quartz create
```

`npx quartz create` 创建项目时，可先选择 `Empty Quartz`，后期再添加要发布的笔记。

三、运行 `npx quartz build --serve`，发布网站，通过 [http://localhost:8080/](http://localhost:8080/) 访问。

![](https://zigholding.github.io/quartz/1694094767556.png)

## 发布已有笔记

`Quartz` 将 `/content` 下的笔记转换为 `/public` 下的 `html` 页面，主页为 `/content/index.md`。`/content` 下的内容有变化时，`Quartz` 能自动识别并发布。

对于已经记录了大量笔记的人而言，重新整理需要发布的笔记是个不小的工作量。

编辑 `quartz.config.ts` 文件，在 `plugins->filters` 下，`Quartz` 默认配置为 `Plugin.RemoveDrafts()`，过滤掉 [Obsidian 元数据](https://zigholding.github.io/quartz/%E7%9F%A5%E8%AF%86%E4%BD%93%E7%B3%BB/Obsidian/Obsidian-%E5%85%83%E6%95%B0%E6%8D%AE) 中 `drafts: true` 的笔记，这种方式会默认发布所有笔记。可以用 `Plugin.ExplicitPublish()` 替换，只发布 `publish : true` 的笔记。

`quartz.config.ts` 中，`ignorePatterns` 也用于定义需要过滤的文件。

-   `some/folder` ：过滤文件夹 `some/folder`
-   `*.md` ：过滤所有带有 `.md` 扩展名的文件
-   `!*.md`：过滤所有没有 `.md` 扩展名的文件
-   `**/private`：过滤任何嵌套级别 `private` 命名的任何文件

其它配置：

-   `pageTitle`：网站标题，点击回到主页

## 发布到 [GitHub Pages](https://zigholding.github.io/quartz/GitHub-Pages)

[GitHub Pages](https://zigholding.github.io/quartz/GitHub-Pages) 是 GitHub 提供的一项功能，用于托管和发布静态网页。它允许用户将他们的代码仓库转化为在线可访问的网站，并可以通过 GitHub 提供的域名（username.github.io）或自定义域名进行访问。

在 [GitHub](https://zigholding.github.io/quartz/GitHub) 上新建项目 `quartz`，点击 `Setting->Pages`，将 `Build and deployment` 下的 `source` 设置为 `Github Action`。

`cd` 到 `quartz` 目录，将 `github` 地址更改为自己的项目地址。

```
git remote remove origin
git@github.com:zigholding/quartz.git
```

[github 配置SSH](https://zigholding.github.io/quartz/github-%E9%85%8D%E7%BD%AESSH)，运行 `npx quartz sync` 同步。

现在，就可以在 [zigholding.github.io/quartz](https://zigholding.github.io/quartz/) 访问了。