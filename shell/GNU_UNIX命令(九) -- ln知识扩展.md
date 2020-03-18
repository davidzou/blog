ln
====

***

### 描述

[Linux命令之ln](GNU_UNIX(九) -- ln.md)一文仅对命令做了参数的介绍。其中还有相关知识点和一些技巧在这里补充。

***

### 为何要使用链接文件？

比如在init.d/目录下有许多用于启动、停止系统服务的脚本，像`httpd`，`cron`,`syslog`等。这些脚本可接受一个自变量，start（代表启动服务）或者stop（代表停止服务）。但是另外Linux系统设计了一个额外的目录机制，就是我们看到的rc0.d到rc6.d等7个目录，每个目录相应于一个运行级。对于某个级别的启动的系统服务，就要在对应的rcX.d目录下设置相应的链接，指向init.d/目录下的启动脚本。链接所占用的磁盘空间更小，也更容易管理，每次变更只需要修改init.d目录下的本尊就好。

***

### 知识扩展

* Linux提供了两种类型的链接方式：软链接和硬链接
    * 软链接（soft link 或者被称为 symbolic link, 有时看到symlink也是它）。这种链接的方式其实只是一个文件名的指针而已，更具体的说，软链接本生也是文件，其内容是另一个文件的名称!当Linux开启软链接时，他会循着指针找出含有实际数据的目标文件。它可以指向另一个文件系统（本机或者远程都可以），也可以指向目录。在`ls -l`查看时，列表中第一个字段标识“l”字样的就是软链接。软链本身的权限没有任何保护措施，所有权都是开放的，但是目标文件的权限仍不变。
        ```
        zzw:temp zzw$ file file1_link1
        file1_link1: ASCII text
        zzw:temp zzw$ ls -l file1_link1
        lrwxr-xr-x  1 zzw  staff  5  3 15 23:20 file1_link1 -> file1
        ^
        |
        这个“l”就是软链接的标识符
        ```

    * 硬链接（hard link）。硬链接其实根本不是“链接”，只是现有文件存于另一个目录下的“入口”而已，也就是说，硬链接与源文件只是以不同名称分居于不同目录下的目录项而已，它们指向同一个innode，具有相同的文件内容、拥有权、访问权限、属性，准确的说是磁盘上的同一块数据罢了。当硬链接被删除时，原文件依然存在；反之亦然，原文件被删除后，硬链接依然存在。对于你而言，除了文件名与目录路径可能不一样之外，根本无法区别硬链接和原文件之间的差异。

        ```
        zzw:temp zzw$ stat -x file1*
          File: "file1"
          Size: 10           FileType: Regular File
          Mode: (0644/-rw-r--r--)         Uid: (  501/     zzw)  Gid: (   20/   staff)
        Device: 1,5   Inode: 69581342    Links: 2
        Access: Thu Mar 19 01:08:30 2020
        Modify: Wed Mar 18 01:50:05 2020
        Change: Thu Mar 19 01:08:28 2020
          File: "file1_hard_link"
          Size: 10           FileType: Regular File
          Mode: (0644/-rw-r--r--)         Uid: (  501/     zzw)  Gid: (   20/   staff)
        Device: 1,5   Inode: 69581342    Links: 2
        Access: Thu Mar 19 01:08:30 2020
        Modify: Wed Mar 18 01:50:05 2020
        Change: Thu Mar 19 01:08:28 2020

        file1 和 file1_hard_link的inode值相同，说明指向同一个磁盘数据。

        zzw:temp zzw$ stat -x file1*
          File: "file1_hard_link"
          Size: 10           FileType: Regular File
          Mode: (0644/-rw-r--r--)         Uid: (  501/     zzw)  Gid: (   20/   staff)
        Device: 1,5   Inode: 69581342    Links: 1
        Access: Thu Mar 19 01:08:30 2020
        Modify: Wed Mar 18 01:50:05 2020
        Change: Thu Mar 19 01:10:50 2020

        当删除file1文件时，这个硬链接还存在，并且Links的值变为了1，表示没有链接的文件了。
        ```

***

### 技巧

* 创建多个文件链接到目录下

    ```
    zzw:temp zzw$ ls -al
    total 24
    drwxr-xr-x   6 zzw  staff  192  3 19 01:26 .
    drwxr-xr-x  31 zzw  staff  992  3 18 17:07 ..
    -rw-r--r--   1 zzw  staff    6  3 19 01:25 file1
    -rw-r--r--   1 zzw  staff    6  3 19 01:25 file2
    -rw-r--r--   1 zzw  staff    6  3 19 01:26 file3
    drwxr-xr-x   2 zzw  staff   64  3 19 01:26 src
    ```

    若要想在`src`目录下创建`file1`,`file2`,`file3`文件的链接文件，一般可能会这样`ln -s file1 file2 file3 src/`, 这样出现了下面的问题。

    ```
    zzw:temp zzw$ ln -s file1 file2 file3 src/
    zzw:temp zzw$ ls src
    file1 file2 file3
    zzw:temp zzw$ ls -al src
    total 0
    drwxr-xr-x  5 zzw  staff  160  3 19 01:29 .
    drwxr-xr-x  6 zzw  staff  192  3 19 01:26 ..
    lrwxr-xr-x  1 zzw  staff    5  3 19 01:29 file1 -> file1
    lrwxr-xr-x  1 zzw  staff    5  3 19 01:29 file2 -> file2
    lrwxr-xr-x  1 zzw  staff    5  3 19 01:29 file3 -> file3
    zzw:temp zzw$ cat src/file1
    cat: src/file1: Too many levels of symbolic links
    ```

    问题就在于当多个文件创建的时候需要使用绝对路径。

    ```
    zzw:temp zzw$ ln -s "$PWD"/file1 "$PWD"/file2 "$PWD"/file3 src/
    zzw:temp zzw$ ls -al src
    total 0
    drwxr-xr-x  5 zzw  staff  160  3 19 01:35 .
    drwxr-xr-x  6 zzw  staff  192  3 19 01:32 ..
    lrwxr-xr-x  1 zzw  staff   46  3 19 01:35 file1 -> /Users/zzw/Develop/workspace/github/temp/file1
    lrwxr-xr-x  1 zzw  staff   46  3 19 01:35 file2 -> /Users/zzw/Develop/workspace/github/temp/file2
    lrwxr-xr-x  1 zzw  staff   46  3 19 01:35 file3 -> /Users/zzw/Develop/workspace/github/temp/file3
    zzw:temp zzw$ cat src/file1
    file1
    zzw:temp zzw$

    ```
