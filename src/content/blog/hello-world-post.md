---
pubDatetime: 2022-09-23T15:22:00Z
modDatetime: 2024-12-21T09:12:47.400Z
title: Hello World Post
slug: hello-world-post
featured: true
draft: false
tags:
  - docs
description:
  Some rules & recommendations for creating or adding new posts using AstroPaper
  theme.
---

## Metadata

| field       | value                 | description |
| :-------- | :-------: | :-------: |
| title       | string                | 显示标题  |
| author      | string                | 作者（默认为 SITE 的值）|
| pubDatetime | 2024-12-01T09:32:31Z  | 发布时间 |
| slug        | string                | filename (as url)     |
| featured    | true/false            | 是否显示在首页feature中 |
| draft       | true/false            | 是否为草稿（不显示）|
| tags        | - xxx                 | 标签 |
| description | string                | (必填) 描述 |

title、author、description 是必填项，字符开头，不支持latex语法。

Here is the sample frontmatter for a post.

```yaml
# src/content/blog/sample-post.md
---
title: The title of the post
author: your name
pubDatetime: 2022-09-21T05:17:19Z
slug: the-title-of-the-post
featured: true
draft: false
tags:
  - some
  - example
  - tags
ogImage: ""
description: This is the example description of the example post.
canonicalURL: https://example.org/my-article-was-already-posted-here
---
```

## Updating Package Dependencies

全局执行下面的命令就可以安装更新，但是最好还是先检查一下有什么 package 是可以更新的：

```bash
npm install -g npm-check-updates
ncu // 检查可以更新的 package
```

大多数情况下，补丁依赖项可以在不影响项目的情况下进行更新。因此，我通常通过运行 `ncu -i --target patch` 或 `ncu -u --target patch` 来更新补丁依赖项。区别在于 `ncu -u --target patch` 会更新所有补丁，而 `ncu -i --target patch` 会让你选择要更新哪些包。你可以根据自己的喜好决定采用哪种方法。

接下来的部分涉及更新次要依赖项。次要包的更新通常不会破坏项目，但最好还是检查相应包的发行说明。这些次要更新通常包含一些可以应用于我们项目的新功能。

```bash
ncu -i --target minor
```

最后，可能有一些主要依赖项的更新。因此，可以通过运行以下命令来检查剩余的依赖项更新：

```bash
ncu -i
```

如果有任何主要更新（或者还有一些需要你手动更新的），上述命令将会输出那些剩余的包。如果包是主要版本更新，你需要非常小心，因为这很可能会破坏整个项目。因此，请仔细阅读相应的发行说明或文档，并据此进行更改。

如果你运行了 ncu -i 并且发现没有更多的包需要更新，则项目中所有的依赖项都更新成功了。

## 更新 Astro Paper 的模板

首先确保不要覆盖那些更新过的文件和目录，比如 src/content/blog/、src/config.ts、src/pages/about.md 以及其他资源和样式文件如 public/ 和 src/styles/base.css，如果没有对模板进行什么修改，那就可以直接替换其他所有的文件为最新的。可以手动替换文件，或者使用 git merge 进行修改。

### 使用 git 更新 (将 Astro Paper 仓库作为 skeleton 使用)

将 Astro Paper 添加为远程仓库；

```bash
git remote add astro-paper https://github.com/satnaing/astro-paper.git
```

先切换到另一个分支进行模板的更新：

```bash
git checkout -b build/update-astro-paper
```

然后拉取 skeleton 模板：

```bash
git pull astro-paper main
git pull astro-paper main --allow-unrelated-histories // 可能存在的报错处理
```

然后处理 merge 的冲突，并将更新分支合并到主分支，再推送到 github。

## 数学公式

$\LaTeX$


