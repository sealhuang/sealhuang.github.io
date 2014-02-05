---
layout: post
title: CentOS 系统 fstab 错误，无法启动的修复方法
category: technique
description: Linux 系统管理常见问题之一。
---

之前在系统上挂了一块网络硬盘，今天突然出了毛病，导致检查系统磁盘时出了错误，启动不了了。错误信息如下：

    Unexpected inconsistency; Run fsck Manually
    ...
    *** An error occurred during the file system check.
    *** Dropping you to a shell; the system will reboot
    ...
    Give root password for maintenance
    (or type Control-D for normal startup):

当然，发现问题在哪首先想到的是修改 /etx/fstab，不过发现此时该文件是 read only。查了点资料，发现解决方法如下：

- 首先重启系统，之后在 Grub 处修改启动配置，在 kernel 那一行最后面加一个 Single，之后启动系统。
- 之后进入系统，还会有之前的错误，在这里输入root密码，运行命令
    mount -n -o remount
- 再然后就可以顺利的修改 fstab，之后重启，正常进入系统。
