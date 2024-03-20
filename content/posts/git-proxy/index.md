---
title: "设置Git代理"
date: 2019-11-19T22:36:40+08:00
draft: true
tags: 
- git
---



之前一直没用 `git` 来下一些大的项目，直到某次更新插件，20k/s 的速度也太慢了一些，改 `host` 只是临时解决方案，最终还是要走上代理的道路上来。

<!--more-->

总的来说有三种方法：

- proxychains
- git config 本身

本来想用`proxychains`的，但是有些现成的脚本还是不太愿意再去修改，就干脆选择第二种设置`git`全局代理

```bash
$ git config --global http.proxy socks5://127.0.0.1:1080
$ git config --global https.proxy socks5://127.0.0.1:1080
```

取消也很简单，都是 git 常规操作

```bash
$ git config --global --unset http.proxy
$ git config --global --unset https.proxy
```