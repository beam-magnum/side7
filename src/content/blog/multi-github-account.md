---
title: 多个 GitHub 账号在同一台电脑上的管理
author: gingerbeam
pubDatetime: 2025-03-04T19:06:31Z
slug: multi-github-account
featured: false
draft: false
tags:
  - git
description:
  如何在一个系统上使用多个 GitHub 账号。
---

新的 GitHub 账号是从前年开始启用的, 原因是觉得旧的账号里面东西太多太乱, 于是干脆重开. 预想是注册一个小号, 把诸如 fork 的课程实验仓库和写日志的仓库都放在一个账号中, 把真的写点代码的仓库放在另一个账号中, 这样大号就比较干净了.

平时自己学习和写东西的话还是用小号多一点, 但是用到大号的时候就发现, 由于 GitHub 已经 ban 了在 cli 使用账号密码登陆的方式, 所以我需要 ssh 或者 token 来 `git push`, 所以没法在本地目录设置 `user` 和 `email`. 推送的时候会报错. 总之应该是我没搞清楚 GCM 的原因.

由于这个设置用的不多, 所以写一篇日志记录一下, 防止以后有一段时间不用导致忘了.

## Naive Solution

在 remote 中把 git 链接改成 `http://<user-name>:token@github.com/xxx/xxx.git`. 每次都要改, 并且 token 明文存在 `.git/config` 文件里. (以前不成熟的时候喜欢用)

## Windows

执行 `git credential-manager store` 之后, 输入的用于登陆的用户名和口令就会被存储在 windows 的凭证管理器中 (比osx的keychain好得多). GitHub 取消密码登陆后, 这个地方的 password 就可以填 token 了, 使用别的 git 服务就正常填写 password 即可.

## 配置 `.git-credential` 文件

在 home 文件夹下的 `.git-credential` 文件会在配置 `git config credential.helper store` 命令时生成. 存储一行 username-password: `https://username:password@github.com`.

光指定没有用, 还需要设置 `git remote set-url origin https://username@github.com/username/repo.git`. 使用差异化 url 指定仓库具体需要使用哪个凭证.

但是这个文件就是丢在 home 下的, 相当于还是在明文存储.

如果是 windows 就没有这个问题, 直接没有这个文件, 在没有差异化 url 指定 token 的情况下, GCM 会直接弹出一个窗口进行选择.

## ssh

ssh 的配置文件在 `~/.ssh/config` 里面, 可以设置不同的 HOST, 都每一个 HOST, 会去调用不同的 ssh 私钥. 也就是说, 你需要给同一个域名 github.com 设置不同的 HOST, 然后绑定不同的私钥文件, 然后每次用 ssh 进行 git push, 并在 git remote 中手动设置 `git remote set-url origin git@HOST:username/repo.git`. 这个操作的麻烦之处在于, 需要对每一个 repo 设置一下 ssh url 地址, 从而区分.

其实用 ssh 也挺好的, 但是也有一些小问题, 比如 ssh 的密钥需要保留在本地, 公钥需要导入到 github 中等, 并且不能实现细粒度控制 (虽然没什么用).
