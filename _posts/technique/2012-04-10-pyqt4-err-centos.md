---
layout: post
title: CentOS 5 下 PyQt4 QFileDialog.getOpenFileName segmentation fault 错误的解决方案
category: technique
description: PyQt 编程问题。
---

昨晚折腾这个问题到深夜，多谢 [XTao][xtao_douban] 同学帮忙解决。具体解决方法如下：

-------------------------

一次帮别人在Linux下面Debug的经历

朋友的系统是 CentOS，正在用 PyQt4 做 GUI 的东西，结果碰到 SegmentFault，一时搞不定，于是我帮他 debug 了一下，最后成功解决问题。

情况：

用的是 Python 开发，已经确定 SegmentFault 发生在执行 QFileDialog.getOpenFileName() 这个函数，后面变死掉了。


首先用的 Python，本来 Python 可以提供 debug 信息的，无奈开发 GUI 的时候，一下子 SegmentFault，没有任何提示信息。这样找不到任何错误信息。

这个时候惟有 GDB 可堪大用，于是用 GDB 开始 Debug Python 程序，使用 ”gdb python” 进入 gdb，输入 ”rfilename.py”，开始执行程序。

弹出 GUI 界面，紧接着运行到 Crash 的地方，gdb 显示了错误信息：

    #0 0x00002aaaab5e7e0a in strncmp () from /lib64/libc.so.6

但是还是不够明确，而且 Qt 使用了 Thread，需要打开 Thread 的所有 backstrace 显示，运行 gdb 命令”threadapply allbt”，这下子好了，所有信息都出来了，一下子可以确定是 glibc 或者 libxml2 的问题，在去网上搜一下，看到了同样的问题，其中有两个帖子信息很重要：[链接1][link_1]和[链接2][link_2]。

这样问题就归到 libxml2 和 libz 上面了，于是按照1上 sahmes 的建议尝试不同版本的 libxml2 和 libz，于是问题解决了，SegmentFault 不见了。

[xtao_douban]: http://www.douban.com/people/xtaooooo/ "Douban of XTao"
[link_1]: http://bugs.centos.org/view.php?id=4931 "bug report 1"
[link_2]: http://answerpot.com/showthread.php?2333093-libxml2+crash "bug report 2"
