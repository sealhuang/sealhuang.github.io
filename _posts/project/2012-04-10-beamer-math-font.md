---
layout: post
title: 在 Beamer 中如何使用标准数学字体
category: project
description: Latex 有时也是相当折腾人。
---

最近经常需要用 Latex 来写文档、做幻灯片，中间穿插着很多公式，但总觉得公式中的字体不是很舒服。调用 amsfonts 包后，也没发现数学符号的字体有什么变化，料想应该是 beamer 中的字体设置出了问题。

仔细查了查，发现 beamer 中默认使用 SansSerif 字体，即没有衬线的字体。如果要在公式中使用标准的 Latex 字体，即 ComputerModern 字体的话，需要在前面加上

    defmathfamilydefault{rmdefault}

之后即可正常使用优美的数学字体了。
