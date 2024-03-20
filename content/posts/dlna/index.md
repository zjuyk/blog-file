---
title: "DLNA 服务的搭建"
date: 2019-12-26T01:49:09+08:00
draft: false
toc: true
tags: 
- dlna
- kodi
---

手机上的电影或者图片有时候需要投影到电脑上展示，虽然 `DLNA` 已经有这么老了，现在无线投屏才是主流，但是由于硬件适配等原因存在着蛮多的问题。我就想着用 `kodi` 来解决手机无线投屏媒体到电脑的问题。
<!--more-->

### 1 环境

- ArchLinux
- KDE
- Kodi

### 2 安装

```bash
$ sudo pacman -S kodi
```

### 3 使用

#### 3.1 构建局域网

我的无线网卡支持热点，所以直接用 `create_ap` 脚本来创建一个 `wifi`

```bash
sudo create_ap wlo1 ppp0 SSID PASSWORD
```

- wlo1: 无线网卡接口名
- ppp0： 我是 L2TP 连接的网络，所以是这个接口，你可以换你的有线接口

#### 3.2 开启UPnP

在 `Settings -> Services -> UPnP` 里开启 `Allow control of Kodi via UPnP` 选项

### 4 手机投屏测试

手机连接 `Wifi`，在任意 `DLNA` 客户端就可以发现电脑设备

**Hint：** 注意必须先打开 `Kodi`

### 参考

1. [UPnP Client](https://kodi.wiki/view/UPnP/Client)
2. [Create Ap](https://github.com/oblique/create_ap)