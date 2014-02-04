---
layout: post
title: Linux 上使用 kingate 搭建 socket 代理服务器
category: technique
description: socket 代理服务器配置。
---

## kingate 简介
kingate 是一位国人开发的代理服务器，支持 http,socks,ftp 等多种协议。支持多线程、tcp 端口映射、规则控制、时间控制、用户认证、http 管理等功能。其官网为[链接][kingate_link]。


## 安装
以下以 CentOS 5.5 平台为例。

首先需要安装一些包，具体命令如下：

    yum install make automake gcc gcc-c++

之后下载kingate源码并编译安装，命令如下：

    wget http://softlayer.dl.sourceforge.net/project/kingate/kingate/2.0/kingate-2.0.tar.gz
    tar xzf kingate-2.0.tar.gz
    cd kingate-2.0/
    ./configure --prefix=/usr/local/kingate
    make
    make install

## 配置 kingate
修改 /usr/local/kingate/etc/kingate.conf 为以下内容，

    http off
    ftp off
    pop3 off
    smtp off
    telnet off
    socks on
    mms off
    rtsp off
    manage on
    max 2000
    max_per_ip 0
    min_free_thread 3
    http_port 8082
    http_accelerate off
    x_forwarded_for on
    http_time_out 30
    ftp_port 2121
    ftp_time_out 300
    pop3_port 1100
    pop3_time_out 300
    smtp_port 2525
    smtp_time_out 300
    telnet_port 2323
    telnet_time_out 300
    socks_port 20120
    socks_time_out 300
    socks5_user off
    mms_port 1755
    mms_time_out 300
    rtsp_port 5540
    rtsp_time_out 300
    manage_port 8822
    manage_time_out 300
    log_model user
    log_level 0
    log_rotate {0 0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23 * * * }
    log_close_msg on
    mem_min_cache 50m
    mem_max_cache 12m
    disk_min_cache 20m
    disk_max_cache 30m
    use_disk_cache off
    refresh never
    refresh_time 300
    user_time_out 0
    mem_cache 50m
    disk_cache 100m
    max_deny_per_ip 0
    max_queue_thread 15
    min_limit_speed_size 1m
    limit_speed 20k
    max_request 50
    total_seconds 10
    bind_addr
    run_user
    insert_via off

以上配置中，socks on 表示启用socks代理(也可以根据你自己的需求开启其他类型的代理)，socks_port 20120 表示socks代理的端口。manage on 为启用http管理，manage_port 8822 为http管理的端口。`强烈建议修改端口号！`

## kingate 的启动和管理
首先下载启动脚本及添加权限，命令如下：

    wget http://soft.vpser.net/proxy/kingate/kingate.init.d
    mv kingate.init.d /etc/init.d/kingate
    chmod +x /etc/init.d/kingate

之后便可使用命令

    /etc/init.d/kingate start
    /etc/init.d/kingate stop
    /etc/init.d/kingate restart

来开启、关闭或重启kingate服务。

之后即可使用proxifier等软件来使用该socket5代理。

[kingate_link]: http://sourceforge.net/projects/kingate/ "home of kingate"
