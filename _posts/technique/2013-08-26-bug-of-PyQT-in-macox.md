---
layout: post
title: 记录 PyQt 的 QMainWindow 在Mac OSX 中的一个 bug
category: technique
description: PyQt开发中问题之一。
---

已经写好的一个 Qt 界面在 Linux 和 Windows 平台下都运行顺利，当迁移到 Mac OS X 后却出现了问题，这里做下记录。

## 问题描述
界面使用了 QMainWindow 的对象，当界面生成并在屏幕上出现后，再加载一个 QToolBar 时，整个界面会突然消失。

## 解决方案
Google后，发现这是 Qt 本身的一个 bug，具体信息见[链接][L0]。

在链接到的网页上也有相应的解决方案，在PyQt的框架下，具体改变如下：

原代码：

    class MainWindow(QMainWindow):
        def __init__(self, parent=None):
            super(BpMainWindow, self).__init__(parent)
            ....
            ....

修改后：

    class MainWindow(QMainWindow):
        def __init__(self, parent=None):
            if sys.platform == 'darwin':
                super(BpMainWindow, self).__init__(parent, Qt.MacWindowToolBarButtonHint)
            else:
                super(BpMainWindow, self).__init__(parent)
            ....
            ....


[L0]: https://bugreports.qt-project.org/browse/QTBUG-4300 "Adding a QToolBar to a shown QMainWindow will hide the QMainWindow"

