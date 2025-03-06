---
title: "多个 GitHub 账号在同一台电脑上的管理"
description: "如何在一个系统上使用多个 GitHub 账号。"
author: gingerbeam
publishDate: "4 Mar 2025"
tags: ["git"]
updatedDate: 6 Mar 2025
---

前年觉得旧的 GitHub 账号上面内容比较乱, 于是重开了. 新注册了两个账号, 一个小号, 把诸如 fork 的课程实验仓库和写日志的仓库都放在其中, 另外一个 main account, 将比较正式的内容都放在里面. 这样账号就比较干净了, 毕竟 fork 的仓库, 学习用的仓库和日志都放在一个账号中显得不够专业 (bushi.

平时用小号还是多一点, 但是需要写点什么东西的时候就发现, 由于 GitHub 已经 ban 了在 cli 使用账号密码登陆的方式, 所以不能每次 push 都登陆了, 需要 ssh 或者 token. 换句话说, github 在逼你学习使用更安全的 GCM, 快说谢谢微软.

不过由于我会在两个账号之间切换, 所以配置的时候还是遇到一些小问题, 所以写一篇日志记录一下, 防止之后有一段时间不碰这个设置导致忘记了.

## Naive Solution

GitHub 刚刚上线 token 的时候, 我在网上搜的方法: 在 remote 中把 git 链接改成 `http://<user-name>:<token>@github.com/<user-name>/<repo>.git`.

每次 gitpull 一个仓库, 或者创建一个仓库的时候都需要特别设置一下这个 url, 并且 token 明文存在 `.git/config` 文件里, 其实不是很合适.

## 配置 `.git-credential` 文件

在 home 文件夹下的 `.git-credential` 文件会在配置 `git config credential.helper store` 命令时生成. 存储一行 username-password: `https://<username>:<password>@github.com`.

光指定没有用, 还需要设置 `git remote set-url origin https://username@github.com/username/repo.git`. 使用差异化 url 指定仓库具体需要使用哪个凭证.

但是这个文件就是丢在 home 下的, 相当于还是在明文存储, 并且也是要额外配置 remote 的.

## Windows

那有没有什么办法能一劳永逸解决呢? 有的, 兄弟, 有的.

GCM 可以直接调用系统的凭证管理器, 比如 gnome 的 keychain 和别的, 可以参考 [这篇文章](https://www.cnblogs.com/apocelipes/p/14491762.html).

但是 wsl 显然是不太方便的, 好在这是世界上最好的 linux 发行版, 你可以直接用 windows 的凭证存储器, 只需要在 wsl 中设置 `git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/bin/git-credential-manager.exe"` 就能调用 git-for-windows 里面的 GCM, 然后调用 windows 的凭证管理器了.

但是凭证管理器设计的思路是对于同样的 url 只能有一套凭证, 这个时候就可以使用差异化的 GitHub url, 即 `git:https://<user-name>@github.com`, 然后当你 `git push` 到一个一般的 `github.com` 地址的时候, GCM 会直接弹出一个窗口进行选择.

快说谢谢微软.

## ssh

其实网上最多的方法是 ssh, 其实也挺好的, 但是 ssh 需要将密钥保留在本地, 公钥导入在 github 中, 并且不能实现细粒度控制. 所以总的来说是觉得这样做不够优雅.

ssh 的配置文件在 `~/.ssh/config` 里面, 可以设置不同的 HOST, 对每一个 HOST, 会去调用不同的 ssh 私钥. 也就是说, 你需要给同一个域名 github.com 设置不同的 HOST, 然后绑定不同的私钥文件, 然后每次用 ssh 进行 git push, 并在 git remote 中手动设置 `git remote set-url origin git@HOST:username/repo.git`. 相当于用 ssh 的话也是需要对 remote 进行额外的设置的.
