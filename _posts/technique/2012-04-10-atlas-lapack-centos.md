---
layout: post
title: CentOS 下安装 ATLAS 和 LAPACK 库
category: technique
description: Linux 系统配置。 
---

在 Python 环境中，若要完成一般的矩阵和数值计算，Numpy 及 Scipy 这两个 module 当然是不可或缺的。

一般 Python 的 module 安装难度并不大，下载源码后，简单的

    python setup.py install

即可轻松搞定。然而 Numpy 和 [Scipy][scipy_link] 所依赖的 [ATLAS][atlas_link]（Automatically Tuned Linear AlgebraSoftware）和[LAPACK][lapack_link]（Linear AlgebraPACKage）两个库的安装则稍显复杂。查了很多资料，发现还是人家自家网站上的说明比较靠谱（链接[在此][install_link]），写的很详细，感兴趣的同学可以参考下。

[scipy_link]: http://www.scipy.org/SciPy "home of Scipy"
[atlas_link]: http://math-atlas.sourceforge.net/ "home of ATLAS"
[lapack_link]: http://www.netlib.org/lapack/ "home of LAPACK"
[install_link]: http://math-atlas.sourceforge.net/atlas_install/ "install guide"
