---
layout: post
title: 如何在CentOs上安装PyQt
category: technique
description: CentOS 5.X系统默认使用的Qt版本较低，如何选择合适的PyQt版本。
---

因为工作的关系，需要在CentOS 5.5系统上安装PyQt4.

系统自带的Qt4版本为4.7.1，在寻找兼容的PyQt4版本上费了不少周折。记录。

## 兼容的软件版本
    Qt 4.7.1
    SIP 4.15.4
    PyQt 4.10.4

## 安装
从PyQt网站上下载以上版本的PyQt和SIP，Qt可以通过yum安装。

首先安装SIP。

解压缩sip源码包后，进入sip目录内，运行

    python configure.py
    make
    make install

完成安装。

之后，安装PyQt。解压缩源码后，进入目录内，运行

    python configure.py -q /path/to/qt4/bin/qmake
    make
    make install


