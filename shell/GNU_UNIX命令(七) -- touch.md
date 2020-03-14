touch
====

***

### 语法

```
touch [options] files
```

> 变更files的访问时间、修改时间。程序猿常用touch来改变文件的时间戳以制造文件内容曾被修改过的假象，不过更多的平时都是用于创建空文件，而忘记了它的真实作用。

***

### 参数

* `-a` 只将文件的“访问时间”设定为当前时间。
* `-m` 只将文件的“修改时间”设定为当前时间。
* `-c` 如果文件不存在就不创建。
* `-h` 如果文件是链接文件，更改链接本身的时间，而不是更改链接指向的文件的时间。注意-h时默认使用-c，因此不会创建任何新文件。
* `-t` [timestamp] 指定设定的时间戳。timestamp的格式如下：
        ```
        [[CC]YY]MMDDhhmm[.ss]
        以2020年3月12日下午18：00为例，应表示为202003121800。
        ```

***

### Example

* `touch file1`

    当file文件不存在时，创建file1，这个时候ls已经不能满足我们的需要了，请使用stat来查看文件详细信息。
    ```
    zzw:temp zzw$ touch file1
    zzw:temp zzw$ ls -al
    total 0
    drwxr-xr-x   3 zzw  staff   96  3 12 23:23 .
    drwxr-xr-x  31 zzw  staff  992  3 10 00:03 ..
    -rw-r--r--   1 zzw  staff    0  3 12 23:23 file1
    zzw:temp zzw$ stat -x file1
      File: "file1"
      Size: 0            FileType: Regular File
      Mode: (0644/-rw-r--r--)         Uid: (  501/     zzw)  Gid: (   20/   staff)
    Device: 1,5   Inode: 69282173    Links: 1
    Access: Thu Mar 12 23:23:17 2020
    Modify: Thu Mar 12 23:23:17 2020
    Change: Thu Mar 12 23:23:17 2020
    ```

* `touch -a file1`

    真正的用法是这个，设定 **访问时间**。

    ```
    zzw:temp zzw$ touch -a file1
    zzw:temp zzw$ stat -x file1
      File: "file1"
      Size: 0            FileType: Regular File
      Mode: (0644/-rw-r--r--)         Uid: (  501/     zzw)  Gid: (   20/   staff)
    Device: 1,5   Inode: 69282173    Links: 1
    Access: Thu Mar 12 23:25:54 2020
    Modify: Thu Mar 12 23:23:17 2020
    Change: Thu Mar 12 23:25:54 2020
    ```

* `touch -m file1`

    设定 **修改时间**

    ```
    zzw:temp zzw$ touch -m file1
    zzw:temp zzw$ stat -x file1
      File: "file1"
      Size: 0            FileType: Regular File
      Mode: (0644/-rw-r--r--)         Uid: (  501/     zzw)  Gid: (   20/   staff)
    Device: 1,5   Inode: 69282173    Links: 1
    Access: Thu Mar 12 23:25:54 2020
    Modify: Thu Mar 12 23:27:24 2020
    Change: Thu Mar 12 23:27:24 2020
    ```

* `touch -t 202003011800 file1`

    指定自定义时间戳，ls此时是看不到正确时间的。

    ```
    zzw:temp zzw$ touch -t 202003011800 file1
    zzw:temp zzw$ stat -x file1
      File: "file1"
      Size: 0            FileType: Regular File
      Mode: (0644/-rw-r--r--)         Uid: (  501/     zzw)  Gid: (   20/   staff)
    Device: 1,5   Inode: 69282173    Links: 1
    Access: Sun Mar  1 18:00:00 2020
    Modify: Sun Mar  1 18:00:00 2020
    Change: Thu Mar 12 23:29:45 2020
    zzw:temp zzw$ ls -al file1
    -rw-r--r--  1 zzw  staff  0  3  1 18:00 file1
    ```
* `touch -c file2`

    -c 参数不会创建不存在的空文件。

    ```
    zzw:temp zzw$ touch -c file2
    zzw:temp zzw$ ls -al
    total 0
    drwxr-xr-x   3 zzw  staff   96  3 12 23:23 .
    drwxr-xr-x  31 zzw  staff  992  3 10 00:03 ..
    -rw-r--r--   1 zzw  staff    0  3  1 18:00 file1
    ```

* `touch -m file1_link`

    当改变的一个链接文件时，其实并不会修改链接文件，而是修改了文件本身。

    ```
    zzw:temp zzw$ ln -s file1 file1_link
    zzw:temp zzw$ ls -al
    total 0
    drwxr-xr-x   4 zzw  staff  128  3 12 23:34 .
    drwxr-xr-x  31 zzw  staff  992  3 10 00:03 ..
    -rw-r--r--   1 zzw  staff    0  3  1 18:00 file1
    lrwxr-xr-x   1 zzw  staff    5  3 12 23:34 file1_link -> file1
    zzw:temp zzw$ stat -x file1_link
      File: "file1_link"
      Size: 5            FileType: Symbolic Link
      Mode: (0755/lrwxr-xr-x)         Uid: (  501/     zzw)  Gid: (   20/   staff)
    Device: 1,5   Inode: 69282657    Links: 1
    Access: Thu Mar 12 23:34:13 2020
    Modify: Thu Mar 12 23:34:13 2020
    Change: Thu Mar 12 23:34:13 2020
    zzw:temp zzw$ touch -m file1_link
    zzw:temp zzw$ stat -x file1_link
      File: "file1_link"
      Size: 5            FileType: Symbolic Link
      Mode: (0755/lrwxr-xr-x)         Uid: (  501/     zzw)  Gid: (   20/   staff)
    Device: 1,5   Inode: 69282657    Links: 1
    Access: Thu Mar 12 23:34:13 2020
    Modify: Thu Mar 12 23:34:13 2020
    Change: Thu Mar 12 23:34:13 2020
    zzw:temp zzw$ stat -x file1
      File: "file1"
      Size: 0            FileType: Regular File
      Mode: (0644/-rw-r--r--)         Uid: (  501/     zzw)  Gid: (   20/   staff)
    Device: 1,5   Inode: 69282173    Links: 1
    Access: Sun Mar  1 18:00:00 2020
    Modify: Thu Mar 12 23:35:02 2020
    Change: Thu Mar 12 23:35:02 2020
    ```

* `touch -h -m file1_link`

    同样修改link文件，-h的作用显而易见。

    ```
    zzw:temp zzw$ touch -h -m file1_link
    zzw:temp zzw$ stat -x file1_link
      File: "file1_link"
      Size: 5            FileType: Symbolic Link
      Mode: (0755/lrwxr-xr-x)         Uid: (  501/     zzw)  Gid: (   20/   staff)
    Device: 1,5   Inode: 69282657    Links: 1
    Access: Thu Mar 12 23:34:13 2020
    Modify: Thu Mar 12 23:38:02 2020
    Change: Thu Mar 12 23:38:02 2020
    zzw:temp zzw$ stat -x file1
      File: "file1"
      Size: 0            FileType: Regular File
      Mode: (0644/-rw-r--r--)         Uid: (  501/     zzw)  Gid: (   20/   staff)
    Device: 1,5   Inode: 69282173    Links: 1
    Access: Sun Mar  1 18:00:00 2020
    Modify: Thu Mar 12 23:35:02 2020
    Change: Thu Mar 12 23:35:02 2020
    ```
