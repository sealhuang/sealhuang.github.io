---
layout: post
title: cacti 中 mem 数据为空的解决方案
category: technique
description: Linux 系统管理常见问题之一。
---

折腾 cacti 半天多，终于把最后一个问题解决。

安装配置的步骤不赘述了，网上的参考资料很多，可以参考 [CentOS+Cacti 安装配置][ref_1] 及 [Cacti 设置-添加监控主机][ref_2] 这两篇博文。

安装配置好后，可采用 ucd/net SNMPHost 模板获取被监控主机的状态信息。基本设置好后，就可以直接看到图了。但总发现有些主机的内存图中只有 cache 和 buffer 的数据，但 free 的数据始终为 Nan。开始认为是 SNMP 设置有问题，反复检查在监控机上都可以正确的获取信息，最后发现是生成的 rrd 文件中出了问题。

首先执行命令

    rrdtool info host_mem_free.rrd

会有如下信息

    filename = "host_mem_free_127.rrd"
    rrd_version = "0003"
    step = 300
    last_update = 1323403801
    header_size = 2048
    ds[mem_free].index = 0
    ds[mem_free].type = "GAUGE"
    ds[mem_free].minimal_heartbeat = 600
    ds[mem_free].min = 0.0000000000e+00
    ds[mem_free].max = 1.0000000000e+07
    ds[mem_free].last_ds = "62486596"
    ds[mem_free].value = NaN
    ds[mem_free].unknown_sec = 1

请注意

    ds[mem_free].max = 1.0000000000e+07

一行，它表明数据的最大值设置为10G，但我们最后一次获取的数据为62486596，显然已经超过最大值，所以数据显示会为 Nan。

通过命令

    rrdtool tune host_mem_free_127.rrd -amem_free:80000000

可将最大值置为80G，之后就可正确显示数据。（具体信息可以参考 [cached memory not graphing][ref_3]）。

由于 cacti 用户监控负载和内存的图可读性不是很好，这里推荐一个不错的[模板][ref_4]。

[ref_1]: http://www.21andy.com/blog/20100204/1615.html "CentOS+Cacti 安装配置"
[ref_2]: http://blog.csdn.net/hw_libo/article/details/6881480 "Cacti设置-添加监控主机"
[ref_3]: http://forums.cacti.net/post-99056.html&amp;highlight= "cached memory not graphing"
[ref_4]: http://hans.fugal.net/blog/2009/01/10/sensible-graphs-with-cacti/ "cacti template"
