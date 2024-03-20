---
title: "PIP更新问题"
date: 2019-11-19T21:19:22+08:00
draft: true
tags: 
- pip
---



前两天想着把老旧的python包给更新一下，或许维持最新的包已经成为`ArchLinux User`的执念了，但是出现了一些没想到的问题。

<!--more-->

> ERROR: Cannot uninstall ‘xxx’. It is a distutils installed project and thus we cannot accurately determine which files belong to it which would lead to only a partial uninstall.

包的数量不多时我一般是直接`sudo pip install --upgrade xxx` 来解决问题，但是`pip list --outdated`发现了好多包都是旧的，秉持着先探索轮子的精神（其实是懒得写脚本），成功发现了`pip-review`。

```bash
$ sudo pip install pip-review
$ pip-review # 查看可更新
$ sudo pip-review --auto # 直接批量更新
$ sudo pip-review --local --interactive # 交互式更新
```

但是却跳出了开头的那个错误，**旧包太久没更新，连依赖都查不到了**

那么现在有两种解决方案：

1. 不管旧包直接安装新的版本，反正不会有影响

    ```bash
    $ sudo pip install --ignore-installed xxx
    ```

2. 非得把旧包删掉，留着没用

    ```bash
    $ sudo pip install --force-reinstall --no-cache-dir xxx
    ```

为了保险我选择了第一种，但出于磁盘考虑你也可以删除旧包
