---
layout: post
title: 今天你 fsck 了吗？
category: technique
description: Linux 系统管理常见问题之一。
---

磁盘文件系统出了故障，开机检查报错很让人郁闷，但这还不是最差的，如果系统提示你需要运行 fsck manually （without-a or -p）的时候，而你直接这么做，中间不加任何的操作，这才是一切噩梦的开始。

`切记：无论是使用 fsck 还是 e2fsck，在检查前请将要检查的磁盘 umount 掉，切记！`

否则，迎接你的很可能是如下的问题（当然也是有解决办法的）.

`注`：以下内容转自一墙外的 [blog][ref_link]。

-------------

fsck: Couldn't fix parent of node: Couldn't find parent ofdirectory entry

In linux file system, when file system error happens, the firstchoice is to do an fsck of the unmounted file system.

Usually,

    fsck -y /dev/sdxx

at boot time should be able to fix everything, if the FS has notbeen severely damaged.

Occasionally, as reported as Debian bug list #478546 at http://groups.google.com/group/linux.debian.bugs.dist/browse_thread/thread/dc48335d2ca42b66 fsck returns error like:

    fsck 1.40.8 (13-Mar-2008)
    e2fsck 1.40.8 (13-Mar-2008)
    Backup contains a file system with errors, check forced.
    Pass 1: Checking inodes, blocks, and sizes
    Pass 2: Checking directory structure
    Pass 3: Checking directory connectivity
    '..' in /lost+found/#122068977 (122068977) is (0),
    should be /lost+found (11).
    Fix? yes

    Couldn't fix parent of inode 122068977: Couldn't find parentdirectory entry

    Pass 4: Checking reference counts
    Pass 5: Checking group summary information

    Backup: ********** WARNING: Filesystem still has errors**********

    Backup: 14398872/122077184 files (1.4% non-contiguous),
    188901088/244137600 blocks
    
And, what ever you repeat fsck, or use e2fsprogs won’t help aboutthis.

Here I explain the cause of this and prresent a solution asfollowing:

[REASON]

In the messed up FS, an orphaned inode with ZERO data length waslabelled as a directory, and was restored into lost+found/ on thealleged file system.

However, for a valid directory entry, it cannot be ZERO length.  It has to have at least two entriesnamely ‘.’ linking to itself and ‘..’ linking to its parentdirectory.  However, since this directory entryhas zero length, fsck failed creating ‘..’ thus resulted theerror

    “Couldn't fix parent of inode 122068977: Couldn't find parent directoryentry"

and resulted a fail of the fsck.

[SOLUTION]

When fsck is done, mount the FS.  Then cd the lost+found/ directory:

    mount #
    cd #
    cd lost+found

If you do ‘ls -al’, you will see a directory with zero length named#, like here#122068977.

It usually has a weird owner and group number.

You then do the following:

    chown root:root # 
    chmod 755 #

This will make it look normal.

Then

    cd # 
    touch test
    rm test
    cd #

Note you cannot do

    cd ..

still here since ‘..’ does not exist yet.

Then if you do

    ls -al

again, the length of the directory # is no longer 0, but the defaultminimum length of your file system, say 4096, 8192, or 16384,etc.

At this point, you can cd off the mounted filesystem, and umountit:

    cd ~
    umount #

and fsck it:

    fsck -y /dev/

It then will result:

    fsck 1.40.2 (12-Jul-2007)
    e2fsck 1.40.2 (12-Jul-2007)
    was not cleanly unmounted,check forced.
    Pass 1: Checking inodes, blocks, and sizes
    Pass 2: Checking directory structure
    Missing '.' in directory inode .
    Fix? yes

    Setting filetype for entry '.' in ... () to 2.
    Missing '..' in directory inode .
    Fix? yes

    Setting filetype for entry '..' in ... () to 2.
    Pass 3: Checking directory connectivity
    '..' in /lost+found/ () is
    (0), should be /lost+found (11).
    Fix? yes

    Pass 4: Checking reference counts
    Inode 2 ref count is 6, should be 7. Fix? yes

    Inode 11 ref count is 4, should be 3. Fix? yes

    Inode ref count is1, should be 2. Fix? yes

    Pass 5: Checking group summary information

    : ***** FILE SYSTEM WASMODIFIED *****
    : 1882918/91193344 files(0.3% non-contiguous), 111803861/182339584 blocks

This will save you from the risk of running a FS with errors orfully copy the file system elsewhere to recreate it.

-------------

另外，fsck 貌似只是 e2fsck 的一个前端，使用 fsck 检查 ext2/3 文件系统时，和使用 e2fsck 是一样的。

千万不要随便删除 lost and found 中的文件。

[ref_link]: http://bitc.bme.emory.edu/~lzhou/blogs/?p=70 "fsck"
