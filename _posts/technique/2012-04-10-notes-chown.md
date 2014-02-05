---
layout: post
title: 误用 chown 递归操作的困惑
category: technique
description: Linux 系统管理问题之一。
---

lab 的服务器崩了，情急下只好将用户的 home 目录转移到新的位置，因为没有在新的系统上重建用户，也就无法在转移文件时保留文件应有的（文件所属用户、群组等）信息，接下来便有了后面的问题。

假设新的用户目录在 /usr/home/sealhuang，运行下列命令试图更改文件所有者

    cd /usr/home/sealhuang
    chown -R sealhuang:sysuser *

试图用 -R 递归的改变该目录下所有文件的所有者，但如此操作只改变了可见的文件的属性，一些隐藏文件（以 `.` 开头的如 .bashrc, .profile等）的属性却没能做修改。之后便做了如下操作：

    chown -R sealhuang:sysuser .*

现在成功的修改了 `.` 文件的用户属性，但意外地发现 /usr/home 目录下的所有文件的所属用户均被修改了。

其实出现这种结果完全是可以预期的，`.*` 既包含 sealhuang 目录下的所有以 `.` 开头的文件，也包含 `..` 即上一层目录，再利用递归操作自然会有这样的结果。其实正确的解决方法很简单：

    cd /usr/home/sealhuang
    chown -R sealhuang:sysuser .

OK，看到区别了吧，只需要指定当前目录( `.` )，即可把目录下包括隐藏文件的用户属性都做更改，而不会影响其他目录。

话说回来，如果在转移文件之前先在新的系统下建立用户，再在转移文件时保留所属用户的信息，以上问题都可避免，给自己少了不少麻烦。
