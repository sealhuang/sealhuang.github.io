---
layout: post
title: 快速创建一个易用的Python环境
category: blog
description: 如何快速搭建Python环境，以及一个简单但实用的应用
published: true
---

## 如何快速搭建Python计算平台
在[上一篇][last_blog]中，我们介绍了Python的一些优点，以及基于它开发的一系列神经影像数据处理工具。这次，你会发现在自己的电脑上配置一个方便易用的Python环境是多么容易。

上一篇的最后，我提到了Anaconda--一个免费的工业级Python计算平台，它可以帮助我们快速搭建Python环境。接下来，我们开始实战。

首先，我们登录Anaconda的[主页][anaconda_home]，在主页的右上部，可以看到Download Anaconda的按钮（见下图）。
![Anaconda home](/images/install_anaconda/anaconda_home.JPG)

点击下载按钮，会弹出一个界面，让你输入一个邮箱，输入框下面有一个选项框，勾选的话，以后会收到一些广告；这里可以不勾选。输入完毕，页面会跳转到下载页（如下图）。
![Download page](/images/install_anaconda/anaconda_download.JPG)

根据你的系统配置，选择对应的版本下载即可（鉴于大家惯于使用Windows，我在这里也以Windows版本的安装为例，继续以下的说明）；下载大概需要20分钟（具体时间视网速而定）。

下载完毕后，直接双击下载的exe安装文件，安装提示一步步安装即可，中间的一些选项使用默认即可；安装完成后，点击Windows开始按钮，进入所有程序，会看到Anaconda的程序列表（如下图).
![Anaconda programs](/images/install_anaconda/anaconda_list.png)

到这里，我们需要做一些简单的配置了。首先，在程序列表中，选择并打开程序Anaconda Command Prompt，输入以下一行命令，以安装用于读写影像数据文件的程序包。

    pip install nibabel

并敲回车执行这条命令；之后程序会自动从网上下载程序包[nibabel][nibabel_github]，并完成安装。安装完成后，一个可用于神经影像数据分析的Python平台就算搭建好了。

## 测试Python环境是否搭建成功：一个实用的小应用
为了测试我们搭建的环境是成功的，这里我们可以实现一个简单但实用的功能，对环境进行测试，看程序是否可以顺利执行。

有时，我们需要把一些坐标点（体素）在脑图上标出来，但却不知道如何进行操作，例如这个[问题][52brain_quest]。

这里我们把这个问题的需求简单明确一下：

*   假设我们现在有11个坐标点要标在一个脑图上，脑图的图像大小是91x109x91；这里各个体素的坐标不是MNI或Talairach坐标，而是在图像矩阵中的体素位置（这个坐标可以与MNI坐标相互转换）。所有体素的坐标已写在一个csv文本[文件][data_link]里（格式为x,y,z）。

*   所有需要写入脑图的体素值都标为1.

*   结果图像保存为data.nii.gz。

另外，为了方便查看生成的图像是否和我们预期的一致，可以使用FreeROI这个软件来实现 数据可视化，下载地址[如下][freeroi_download]。下载后解压，双击里面的freeroi.exe即可运行使用。这里我们假设FreeROI的安装路径是D:\software\freeroi。

我们将下载得到的csv文件保存为D:\voxel_coord.csv，接下来,在Anaconda的程序列表中，打开程序IPython (Py 2.7)，输入以下命令(每敲完一行要敲回车执行，#开头的为注释，不是命令)：

    # 进入存放数据的D盘
    cd D:\
    import os
    import numpy as np
    import nibabel as nib
    # 输入FreeROI的存放路径，这里默认放到D盘下
    freeroi_path = r'D:\software\freeroi'
    # 输入存放体素坐标的csv文件的路径
    coord_file = r'D:\voxel_coord.csv'
    # Read csv file
    coord_info = open(coord_file).readlines()
    coord_info = [line.strip().split(',') for line in coord_info]
    coord_info = [[int(line[0]), int(line[1]), int(line[2])] for line in coord_info]
    mni_template = os.path.join(freeroi_path, 'data', 'standard', 'MNI152_T1_2mm_brain.nii.gz')
    img = nib.load(mni_template)
    header = img.get_header()
    # generate a new data, data size is 91x109x91
    data = np.zeros((91, 109, 91))
    # write each voxel into data matrix
    for line in coord_info:
        data[line[0], line[1], line[2]] = 1
    # save data matrix as nifti file
    header['cal_max'] = data.max()
    header['cal_min'] = 0
    img = nib.Nifti1Image(data, None, header)
    nib.save(img, 'data.nii.gz')

上面的程序执行完毕后，就会在D盘下发现一个生产的data.nii.gz文件，里面保存了制定坐标的各个体素。

之后，我们可以双击打开freeroi，使用它看下生产的图像是否正确。
打开软件后，首先选择File-Open stnadard volume，打开默认的MNI标准头，这个图像的大小也是91x109x91的；在主界面的左侧，可以选择图像中十字坐标的位置，这里我们设置一个需要标定的体素的位置，如下图：
![coord_config](/images/install_anaconda/coord_config.png)

然后滚动鼠标，在图像上找到十字坐标标注的位置。之后，通过File-Add volume打开我们刚刚制作的data.nii.gz文件，并将它的显示颜色设置为red2yellow，会看到十字坐标指示的位置已经变为黄色，即我们成功地将指定坐标的体素在图像上标注了出来（如下图）。
![image display](/images/install_anaconda/image_disp.png)

看到这里，你是不是觉得自己也get到新技能了呢？ :)


[last_blog]: http://sealhuang.github.io/python-in-neuroimage "Python在脑成像数据分析中的应用"
[anaconda_home]: https://store.continuum.io/cshop/anaconda "Anaconda Scientific Python Distribution"
[nibabel_github]: https://github.com/nipy/nibabel "Github of NiBabel"
[52brain_quest]: http://home.52brain.com/thread-26263-1-1.html "什么软件可以把体素在大脑的具体位置上标出来"
[data_link]: https://raw.githubusercontent.com/sealhuang/python-tutorial/master/data/voxel_coord.csv  "Data"
[freeroi_download]: http://sourceforge.net/projects/freeroi/files/FreeROI-0.2.2/freeroi-win-0.2.2.zip/download "Download FreeROI"
