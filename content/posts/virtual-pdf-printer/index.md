---
title: "PDF 虚拟打印机"
date: 2019-12-10T23:00:45+08:00
draft: false
toc: true
tags: 
- pdf
- printer
---



在 `WIN10` 系统上是自带 `PDF` 虚拟打印机的，于是乎文档转 `PDF` 变得非常容易。在 `ArchLinux` 上我还装了 `WPS` 方便日常工作接收的文档的查看和撰写，由于格式的原因通常需要准备一份 `PDF` 备用，干脆装一下虚拟打印机。

<!--more-->

### 1 安装

目前使用最广泛的打印系统是 `CUPS`，它会在本地启动一个名叫 `cupsd` 的守护进程用来维护打印队列，提供了一个web管理页面（http://localhost:631）。

```bash
$ sudo pacman -S cups cups-pdf #安装cups和pdf虚拟打印机
```

### 2 使用

#### 2.1 启动服务

```bash
$ sudo systemctl start cups.service
```

#### 2.2 开机自启动

```bash
$ sudo systemctl enable cups.service 
```

#### 2.3 添加打印机

我是KDE用户自带了 `print-manager`，如果你是其他桌面的话可以通过 `web` 管理页面自行添加

**Hint:**

- 在 `Make` 里选择 `Generic`
- 在 `Model` 里选择 `（w/ option）`，我之前没选导致打印空白

#### 2.4 修改输出路径

默认输出路径在 `/var/spool/cups-pdf/${USER}`，可以修改 `/etc/cups/cups-pdf.conf`

```
Out /your-path
```

### 3 参考

[ArchWiki](https://wiki.archlinux.org/index.php/CUPS)

