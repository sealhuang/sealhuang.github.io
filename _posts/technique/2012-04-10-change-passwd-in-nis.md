---
layout: post
title: 关于 NIS 在 client 上无法修改密码的问题
category: technique
description: Linux 系统管理常见问题之一。 
---

这几天依然在马不停蹄的配置服务器，为了实现方便地在多台服务器上同步用户列表，还是选择了 NIS。

基本的安装方法网上有很多，相对比较容易搞定；不过最后发现一个问题，就是 master 的数据库可以在 client 上同步，但在 client 上无法用 yppasswd 命令修改密码。最后发现是在 master 上启动 yppasswdd 命令时的配置有误。

正确的启动方法为

    /user/sbin/rpc.yppasswdd -D/etc

之后便可正常修改密码。

这个错误的具体原因可以在 /var/log/message 内得到答案。

昨晚折腾到11点多，中秋节好好休息一下～

中秋快乐！
