---
title: "Gentoo 安装笔记"
date: 2020-12-23T23:44:14+08:00
draft: true
tags:
- gentoo
- install
toc: true
---

实际上这样的安装文章已经很多了，gentoo wiki 在安装部分的叙述也很全面，但还是想自己捋一捋自己的操作流程。

<!--more-->

### 1 我的环境
- Ubuntu 20.10 Live USB （临时用 ventoy 做的启动盘，选用它是因为自带图形界面）
- Windows 10 & Arch Linux 双系统 （只有一个盘，准备三个系统共用 esp 分区）

### 2 预先准备
#### 2.1 分区
  
  使用 `cfdisk` 分出一个 `root` 分区就好，交换分区不单独设，需要的时候再弄个 `swapfile` 。如果有复杂的分区要求，可以使用 ubuntu 自带的 `gparted` 这个工具也非常简单。至于格式由于第一次安装就选了 `ext4` ，没啥其他特别的。

#### 2.2 下载 `stage3`
  
  由于众所周知的原因[官网](https://www.gentoo.org/downloads/)的速度会有点慢，我是挂了梯子。`Stage3`，也就是包含`emerge` 和 `gcc`，`python` 等基础编译构建工具的最小系统，里面不包含内核和 `BootLoader`。一般 PC 都是 `amd64` 架构，其下有好几个选择例如 `openrc`， `no multilib` 等，我选择屈服于 `systemd` 的统治（确实是用习惯了）。


#### 2.3 挂载分区

  ```
  $ mkdir /mnt/gentoo
  $ mount /dev/sda7 /mnt/gentoo
  $ mkdir /mnt/gentoo/boot
  $ mount /dev/sda1 /mnt/gentoo/boot
  ```
  
#### 2.4 解压 `stage3`

  ```
  $ cp ~/Downloads/stage3-*.tar.xz /mnt/gentoo
  $ tar xpvf stage3-*.tar.xz --xattrs-include='*.*' --numeric-owner # 保证权限不出问题
  ```

#### 2.5 新建 `fstab`

  因为我只新建了一个 `root` 分区，简单的写成这样就行了，因为懒也没去换成 `UUID`
  ```
  /dev/sda1   /boot   vfat    defaults,noatime    0 2
  /dev/sda7   /   ext4    noatime         0 1
  ```
#### 2.6 挂载虚拟文件系统

  ```
  # 挂载虚拟文件系统，每次 chroot 都需要
  $ mount --types proc /proc /mnt/gentoo/proc
  $ mount --rbind /sys /mnt/gentoo/sys
  $ mount --make-rslave /mnt/gentoo/sys
  $ mount --rbind /dev /mnt/gentoo/dev
  $ mount --make-rslave /mnt/gentoo/dev
  ```
  因为我不是用 `gentoo` 的 `livecd` 安装而是使用的 `ubuntu live usb`, 所以需要额外设置 `shm`
  ```
  $ test -L /dev/shm && rm /dev/shm && mkdir /dev/shm
  $ mount --types tmpfs --options nosuid,nodev,noexec shm /dev/shm 
  $ chmod 1777 /dev/shm
  ```
#### 2.7 配置 DNS

  `$ cp --dereference /etc/resolv.conf /mnt/gentoo/etc/ `

#### 2.8 配置 `portage`

  修改 `/mnt/gentoo/etc/portage/make.conf`
  ```
  # Compiler flags to set for all languages
  COMMON_FLAGS="-march=native -O2 -pipe"
  # Use the same settings for both variables
  CFLAGS="${COMMON_FLAGS}"
  CXXFLAGS="${COMMON_FLAGS}"
  ```
  其中 `-march=native` 表示自动检测使用的 `CPU` 类型，进行优化编译，`-O2` 表示优化等级， `-pipe` 表示在编译过程中使用UNIX管道而不是临时文件来通讯（保护SSD，但是内存不够会被kill掉）
  
  也可以先额外设置一些参数
  ```
  MAKEOPTS="-j3" # 设置并行，我是双核四线程，留了一个线程跑桌面
  GENTOO_MIRRORS="https://mirrors.tuna.tsinghua.edu.cn/gentoo/" # 设置镜像源
  # 熟练的话也可以提前设好 USE
  ```

#### 2.9 正式安装
  
- 先 chroot 进去
  ```
  $ chroot /mnt/gentoo /bin/bash
  $ source /etc/profile
  ```
- 修改一下终端提示符
  ```  
  $ export PS1="(chroot) ${PS1}"
  ```
- 配置 `ebuild` 软件库
  ```
  $ mkdir --parents /mnt/gentoo/etc/portage/repos.conf
  $ cp /mnt/gentoo/usr/share/portage/config/repos.conf /mnt/gentoo/etc/portage/repos.conf/gentoo.conf
  ```
- 拉取快照
  ```
  $ emerge-webrsync
  ```
- 更新 `world`
  
  会更新蛮多包，建议并行安装 `-jx`
  ```
  $ emerge --ask --verbose --update --deep --newuse @world -j3
  ```
  再安装一些必备的软件包
  ```
  $ emerge -av git vim eix gentoolkit
  ```
- 配置时区和 `locale` 
  
  ```
  $ localectl set-locale LANG=en_US.utf8
  $ timedatectl set-timezone Asia/Shanghai
  $ echo en_US.UTF-8 UTF-8 >> /etc/locale.gen
  $ echo zh_CN.UTF-8 UTF-8 >> /etc/locale.gen
  $ echo zh_TW.UTF-8 UTF-8 >> /etc/locale.gen
  $ locale-gen
  # 重新加载环境
  $ env-update && source /etc/profile && export PS1="(chroot) ${PS1}"
  ```

- 安装二进制内核
  
  第一次安装为了方便选择了二进制内核
  ```
  emerge -av gentoo-kernel-bin --autounmask
  ```
  安装 `firmware` 
  ```
  emerge -av sys-kernel/linux-firmware
  ```

- 设置网络
  
  ```
  $ vim /etc/conf.d/hostname
  # add hostname
  hostname="gentoo"
  ```
  ```
  $ vim /etc/hosts
  # add hosts
  127.0.0.1	localhost
  ::1		localhost
  127.0.1.1	gentoo.localdomain gentoo
  ```
  安装 `networkmanager`
  ```
  $ euse -E networkmanager
  $ emerge -avuDN @world
  $ systemctl enable NetworkManager
  ```
- 新建用户设置密码

  ```
  # 先设置 root 密码
  $ passwd
  
  # 新建用户
  $ useradd -m -G users,wheel,audio,video -s /bin/bash/ zjuyk
  $ passwd zjuyk
  
  # 开启权限
  $ emerge -av sudo
  $ visudo
  ## Uncomment to allow members of group wheel to execute any command
  %wheel ALL=(ALL) ALL
  ```
- 配置引导
  
  由于我是三系统，其中我 `Arch Linux` 已经安装好了 `grub`, 直接在 `Arch Linux`  上 `grub-mkconfig` 一下再将 `grub.cfg` 中的 `gentoo entry`  的 `UUID` 以及内核名等参数修改为实际值就可以完成引导。

### 参考
- [wgjak47's blog](http://wgjak47.me/tech/gentoo-linuxan-zhuang-bi-ji-2020ban-zh_CN.html)
- [Gentoo HandBook](https://wiki.gentoo.org/wiki/Handbook:AMD64) 
