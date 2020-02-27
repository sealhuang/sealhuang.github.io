---
layout: post
title: 如何对Docker的volume文件在主机间迁移
category: technique
description: Docker固然方便部署，但迁移文件也是技术活
published: true
---

## 缘起
迫于疫情，不得不将部署在内网的Jupyter工作环境转而部署在外网上，幸亏有Docker在，docker compose几个命令就完成了迁移工作，却想起用户的数据并没有一并挪过来。查了一些资料，用下面的办法搞定了。

## 解决方案

### 从原主机上导出数据文件

一般Docker的volume文件都保存于`/var/lib/docker/volume`目录下，如果不确定自己的数据保存路径，可以使用`docker volume ls`和`docker volume inspect`命令来查询。

由于/var/lib/docker目录下的文件一般是禁止随意访问的，这里我们还是使用一个docker container对文件进行导出。命令如下：

    docker run --rm -it -v ~/volume-backup:/backup -v /var/lib/docker:/docker busybox tar cfz /backup/volume.tgz /docker/volumes/

上面这个命令中，我们运行了一个轻量级的container——busybox，并挂载了两个目录：

1. (host) ~/volume-backup  =>  (container) /backup

2. (host) /var/lib/docker/ =>  (container) /docker

在container中，我们执行了一个打包的命令，将docker目录下的volume文件打包保存到备份的目录下。

### 在新主机上倒入数据

我们先将volume.tgz文件拷贝到新的主机上，这里假设我们仍保存在和原主机上相同的目录里（`~/volume-backup/volume.tgz`），之后我们需要再次访问`/var/lib/docker`来导入数据。

具体地，我们首先进入保存压缩包的目录，并将文件解压：

    cd ~/volume-backup
    tar zxvf volume.tgz

之后，我们可以使用以下命令来导入名为`#volume-name#`的数据文件

    docker run --rm -it -v /var/lib/docker:/docker -v ~/volume-backup/docker/volumes:/volume-backup busybox cp -r /volume-backup/#volume-name# /docker/volumes

至此，数据迁移完成。

