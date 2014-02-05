---
layout: post
title: Fedora 下安装 dropbox
category: technique
description: Linux 系统配置。
---

以为在 fedora 下安装 dropbox 也很容易，在官网下载下 rpm 包，装上就能用了，但问题总是很诡异的出现。

用 rpm 装好后，启动 dropbox，此时它应该会先与服务器取得联系，但除了一个报错，与 http_proxy 设置有关，查了一下，原来需要再装一个包,下载地址[如下][http://ishare.iask.sina.com.cn/f/16804339.html]。

之后解压到自己的 home 目录里，OK。再运行 dropbox 就可顺利连接了。

`P.S.` dropbox 官网提供的 rpm 包是针对 fc10 进行了优化的，如果觉得不爽，可以下载源码包自己编译下，当然之前你可能还需要再安装系 nautilus-devel 这个包。
