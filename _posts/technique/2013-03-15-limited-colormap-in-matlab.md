---
layout: post
title: MATLAB 的 colormap 在 Windows 系统中颜色数量上限问题
category: technique
description: 再一次发现 MATLAB 与 Windows 的不兼容
---

## 问题描述
最近发现一个比较诡异的问题，假设在 Windows 环境下，运行下面几行 Matlab 代码：

    % 生成一个10X10的随机矩阵
    A = rand(10)；
    % 显示此矩阵
    imagesc(A);
    % 输出colorbar
    colorbar
    % 默认颜色显示，只是将映射区间设为256种颜色。
    imagesc(jet(256))

输出如下：
![256 colormap](/images/matlabcolormap/windows_256.png)

可见，在对应的 colormap 中，0对应蓝色，1对应红色，显示正常。

如果再执行以下代码：

    colormap(jet(300))

显示的颜色就出现了问题：最大值部分又出现了蓝色。
![300 colormap](/images/matlabcolormap/windows_300.png)

将相同的代码在 Linux 等平台下运行，没有出现以上问题。

## 解决方案
实际上这个问题是 Windows 平台下，MATLAB 渲染图像时的一个 bug（至少运行在 Windows 7 下的 2011a and 2012a 都会出现此问题），colormap 的颜色数量被限制为最多256种。

现在已知的解决方案是运行如下代码：

    set(gcf,'renderer','OpenGL')

但在某些电脑下，可能需要额外配置下 OpenGL。
