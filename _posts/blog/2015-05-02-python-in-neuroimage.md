---
layout: post
title: Python在脑成像数据分析中的应用
category: blog
description: 开篇：你为什么需要Python，以及一些实用的Python工具包
published: true
---

脑成像技术已经成为认知科学和心理学研究领域中一种重要的研究手段，帮助研究者不断深入发掘我们脑中的秘密。伴随着研究的不断深入，各式各样的指标参数和分析方法也不断推陈出新，以迅雷不及掩耳之势进入了我们的视野。如何用它们来完善和深入我们自己的研究，成为多数研究者共同的问题。

很多时候，一个“好”的指标或方法并不会很难理解甚至晦涩，它背后的意义也被提出者以相对清晰的方式传达给大家；此时真正的问题是如何去用，或者在没有现成软件的情况下，怎么去实现它。编程，就成了一个躲不开的问题。

如果你正面临这样的问题，同时又恰好具备以下特点：

*   没钱、没人脉，找不到能帮我写程序的人（有程序猿男朋友的妹子们可以不用往下看了......）；

*   不是“让我编程我会死星”人；

*   硬着头皮可以写些代码，但希望代码可以和文字一样易读易写；

*   希望代码量尽可能小，最好两三行代码就能解决问题；

*   希望程序语言的工具包多，可以得到很多技术支持；

另外，如果恰好你是A型血处女座，希望代码写出来格式规整（光是看着就能赏心悦目），那么不要停，继续看下去，你不会失望的！

Python可以满足以上所有要求.
![Python logo](/images/python/Python_logo.png)

[Python][python_home]是一种通用的计算机程序设计语言，不仅可以进行数值处理，也可以轻松进行字符串处理、文件操作等应用，它以语法简洁清晰见长，具有丰富和强大的类库。它强调使用者的生产力，让用户把精力集中在逻辑上而不是语言本身。另外，它也常被称为“胶水语言”，能够把其他语言制作的各种程序模块很轻松的联接在一起。

这些特点都很符合一个研究者的需求：

*   对于经典的数学方法，有丰富的现成工具：我们不希望重新编写程序去画曲线、傅立叶变换或者拟合算法。不要重复发明轮子！

*   易于学习：计算机科学不是我们的工作也不是我们的教育背景。我们想要在几分钟内画出曲线，平滑一个信号或者傅立叶变换。

*   可以方便的与其他人交流：代码的可读性像书一样。Python这种语言包含了尽可能少的语法符号或不必要的编程规定。

*   在一个单一的语言/环境中做所有的事，避免每个新问题都要学习新软件。

*   Python是完全免费的。Matlab一般是要收费的，当然，在天朝这个可能不是一个问题...

当前，Python在脑成像研究领域已经有了很多应用，这里简单列举一些相对完善的工具包。

*   PsychoPy
    [PsychoPy][psychopy_home]是一个用于刺激呈现的工具包，适用于心理物理、脑成像和其他心理学实验。它提供一整套方便的图形交互界面，帮助研究者设计实验流程，确定各类实验刺激（图片、视频或音频等）的呈现时间和时长，并记录被试的反应情况（按键和反应时）。

    ![psychopy logo](/images/python/psychopy_logo.png)
    ![psychopy GUI](/images/python/psychopy_gui.png)

*   NiBabel
    [NiBabel][nibabel_home]提供了对脑成像数据的读写接口，支持多种数据格式，如ANALYZE（plain，SPM），GIFTI，NIfTI，MINC和PAR/REC文件。可以说，NiBabel是python在脑成像领域应用的重要基石。

    ![nibabel logo](/images/python/nibabel_logo.png)

*   DiPy
    [DiPy][http://nipy.org/dipy]提供了一整套DTI数据处理方法，可以方便地进行白质纤维确定性/概率性追踪，并计算各种基于DTI数据的指标。

    ![dipy logo](/images/python/dipy_logo.png)

*   NiPy
    [NiPy][nipy_home]提供了一系列用于功能脑成像数据处理/分析的工具，包括常用的基于一般线性模型（General Linear Model，GLM）的统计分析，silce timing，motion correction，以及图像配准等分析处理。

    ![nipy logo](/images/python/nipy_logo.png)

*   Nipype
    [Nipype][nipype_home]是一个用于整合当前各种主流分析软件的工具包，通过它可以将SPM，FSL，FreeSUrfer，AFNI等软件中的各个处理程序，连接成一套个人订制的数据分析处理流程，给予研究者最大的自由度，根据自己的需要使用软件。Nipype在进行大批量数据处理，尤其是调用并行处理机制上具有独特的优势。

    ![nipype logo](/images/python/nipype_logo.png)

*   PyMVPA
    当前多体素分析（Multi-Voxel Pattern Analysis, MVPA）已经成为了一种新的数据分析潮流，而来自Haxby实验室的这款[PyMVPA][pymvpa_home]工具包则为研究者进行MVPA分析提供了便利，通过它可以方便的调用多种多元方法对数据进行分析，如构建分类器，searchlight分析等。

    ![pymvpa logo](/images/python/pymvpa_logo.png)

*   Scikit-learn
    [Scikit-learn][scikit_learn_home]是一个当前很流行的机器学习工具包，提供了多种在模型训练、测试时必需的分析方法和工具，可以轻松实现各种分类器（如SVM，决策树，随机森林等）。

    ![sklearn logo](/images/python/sklean_logo.png)

*   FreeROI
    [FreeROI][freeroi_home]是一个基于Python的多用途图像处理软件，提供多种图像分割（如分水岭分割和区域增长等）和图像处理（如平滑，二值化等）工具，并针对确定感兴趣区域（Region of Interest，ROI）提供多种方便实用的实现方案，可方便的由全脑激活图提取ROI，或根据坐标生成规则形状的ROI（如小球或立方体）。同时，软件提供了一个非常人性化的图形操作界面，以所见即所得的形式完成所有操作。

    ![freeroi logo](/images/python/freeroi_logo.png)
    ![freeroi GUI](/images/python/freeroi_gui.png)

*   PySurfer
    [PySurfer][pysurfer_home]是一个数据可视化工具，可以将FreeSurfer软件产生的数据在大脑皮层上进行可视化，并提供多种可视化角度和配色方案，是一个很方便和高效的可视化/作图工具。

    ![pysurfer logo](/images/python/pysurfer_logo.png)

以上介绍了一些在脑成像研究领域常用的工具包。当然，要使用这些包，我们还需要在自己的电脑上安装Python环境。这里介绍一个很方便的安装方案--Anaconda。Anaconda是一个Python的科学计算平台，包含了大量用于数值计算和拘束分析的开源程序包（modules）。Anaconda的优势有安装简单，并且工具齐全，且有面向科研的免费版。整个环境安装简单。具体使用方法会在之后具体介绍。

[python_home]: http://www.python.org "Python.org"
[psychopy_home]: http://www.psychopy.org "PsychoPy.org"
[nibabel_home]: http://nipy.org/nibabel "Home of NiBabel"
[dipy_home]: http://nipy.org/dipy "Home of DiPy"
[nipy_home]: http://nipy.org/nipy "Home of NiPy"
[nipype_home]: http://nipy.org/nipype "Home of Nipype"
[pymvpa_home]: http://www.pymvpa.org "Home of PyMVPA"
[scikit_learn_home]: http://scikit-learn.org "Home of Scikit-learn"
[freeroi_home]: http://freeroi.brainactivityatlas.org "Home of FreeROI"
[pysurfer_home]: http://pysurfer.github.io "Home of PySurfer"

