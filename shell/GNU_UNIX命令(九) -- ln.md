ln
====

***

### 语法

```
ln [options] source_file target_file
```

> 不同平台下的命令参数不一样，这里以Mac为例。具体参数参考具体平台。

***

### 参数

* `-F`    如果目标文件已经存在并且是一个目录，请将其删除，以便可以进行链接。`-F`选项应该与`-f`或`-i`一起使用。如果未指定，则隐含`-f`，除非指定了`-s`选项，`-F`选项是no-op。

* `-h`    如果目标文件或目标目录是符号链接，请不要指向它。这对于`-f`选项非常有用，可以替换可能指向目录。

* `-f`    如果目标文件已经存在，则取消链接，以便可以进行链接。（`-f`选项将覆盖以前的任何`-i`选项。）

* `-i`    如果目标文件存在，则导致`ln`将提示写入标准错误。如果标准输入的响应以字符`y`或`y`开头，则取消链接目标文件，以便可以进行链接。否则，不要尝试链接。（`-i`选项将覆盖以前的任何`-f`选项。）

* `-n`    与`-h`相同，以与其他`ln`实现兼容。

* `-s`    创建一个链接文件。

* `-v`    显示创建链接的详细信息。

> `-h`, `-i`, `-n` 和 `-v`选项是非标准的不推荐在Shell脚本中使用。它们的存在只是为了版本的兼容。

***

### Example

* `ln -s file1 file1_link`

    创建一个链接文件。

    ```
    zzw:temp zzw$ ls
    file1 src
    zzw:temp zzw$ ln -s file1 file1_link
    zzw:temp zzw$ ls -al
    total 8
    drwxr-xr-x   5 zzw  staff  160  3 15 23:03 .
    drwxr-xr-x  31 zzw  staff  992  3 15 02:04 ..
    -rw-r--r--   2 zzw  staff    6  3 12 23:56 file1
    lrwxr-xr-x   1 zzw  staff    5  3 15 23:03 file1_link -> file1
    drwxr-xr-x   3 zzw  staff   96  3 15 22:52 src
    zzw:temp zzw$ stat -x file1_link
      File: "file1_link"
      Size: 5            FileType: Symbolic Link
      Mode: (0755/lrwxr-xr-x)         Uid: (  501/     zzw)  Gid: (   20/   staff)
    Device: 1,5   Inode: 69361057    Links: 1
    Access: Sun Mar 15 23:03:29 2020
    Modify: Sun Mar 15 23:03:29 2020
    Change: Sun Mar 15 23:03:29 2020
    ```

* `ln -f file1 file1_link`

    取消一个软链，此时软链文件就变为了一个标准文件，你可以继续使用它。

    ```
    zzw:temp zzw$ ln -h file1 file1_link
    ln: file1_link: File exists
    zzw:temp zzw$ ls -al
    total 8
    drwxr-xr-x   5 zzw  staff  160  3 15 23:03 .
    drwxr-xr-x  31 zzw  staff  992  3 15 02:04 ..
    -rw-r--r--   2 zzw  staff    6  3 12 23:56 file1
    lrwxr-xr-x   1 zzw  staff    5  3 15 23:03 file1_link -> file1
    drwxr-xr-x   3 zzw  staff   96  3 15 22:52 src
    zzw:temp zzw$ ln -f file1 file1_link
    zzw:temp zzw$ ls -al
    total 16
    drwxr-xr-x   5 zzw  staff  160  3 15 23:07 .
    drwxr-xr-x  31 zzw  staff  992  3 15 02:04 ..
    -rw-r--r--   3 zzw  staff    6  3 12 23:56 file1
    -rw-r--r--   3 zzw  staff    6  3 12 23:56 file1_link
    drwxr-xr-x   3 zzw  staff   96  3 15 22:52 src
    zzw:temp zzw$ stat -x file1_link
      File: "file1_link"
      Size: 6            FileType: Regular File
      Mode: (0644/-rw-r--r--)         Uid: (  501/     zzw)  Gid: (   20/   staff)
    Device: 1,5   Inode: 69282173    Links: 3
    Access: Sun Mar 15 23:07:51 2020
    Modify: Thu Mar 12 23:56:25 2020
    Change: Sun Mar 15 23:07:50 2020
    ```

    如果不添加参数 file1 直接使用链接文件名，效果就有点不一样了。**被直接删除了**。

    ```
    zzw:temp zzw$ ln -f file1_link
    ln: ./file1_link: No such file or directory
    zzw:temp zzw$ ls -al
    total 8
    drwxr-xr-x   4 zzw  staff  128  3 15 23:12 .
    drwxr-xr-x  31 zzw  staff  992  3 15 02:04 ..
    -rw-r--r--   2 zzw  staff    6  3 12 23:56 file1
    drwxr-xr-x   3 zzw  staff   96  3 15 22:52 src
    ```

* `ln -Fis file1 file1_link`

    创建软链，存在则提示。

    ```
    zzw:temp zzw$ ls -al
    total 16
    drwxr-xr-x   5 zzw  staff  160  3 15 23:07 .
    drwxr-xr-x  31 zzw  staff  992  3 15 02:04 ..
    -rw-r--r--   3 zzw  staff    6  3 12 23:56 file1
    -rw-r--r--   3 zzw  staff    6  3 12 23:56 file1_link
    drwxr-xr-x   3 zzw  staff   96  3 15 22:52 src
    zzw:temp zzw$ ln -F file1 file1_link
    ln: file1_link: File exists
    zzw:temp zzw$ ln -Fis file1 file1_link
    replace file1_link? c^C
    zzw:temp zzw$ ln -Fi file1 file1_link
    replace file1_link? y
    zzw:temp zzw$ ls -al
    total 16
    drwxr-xr-x   5 zzw  staff  160  3 15 23:10 .
    drwxr-xr-x  31 zzw  staff  992  3 15 02:04 ..
    -rw-r--r--   3 zzw  staff    6  3 12 23:56 file1
    -rw-r--r--   3 zzw  staff    6  3 12 23:56 file1_link
    drwxr-xr-x   3 zzw  staff   96  3 15 22:52 src
    zzw:temp zzw$ ln -Fis file1 file1_link
    replace file1_link? y
    zzw:temp zzw$ ls -al
    total 8
    drwxr-xr-x   5 zzw  staff  160  3 15 23:10 .
    drwxr-xr-x  31 zzw  staff  992  3 15 02:04 ..
    -rw-r--r--   2 zzw  staff    6  3 12 23:56 file1
    lrwxr-xr-x   1 zzw  staff    5  3 15 23:10 file1_link -> file1
    drwxr-xr-x   3 zzw  staff   96  3 15 22:52 src
    ```

* `ln -sv file1 file1_link1`

    看看`-v`参数的效果。

    ```
    zzw:temp zzw$ ln -sv file1 file1_link1
    file1_link1 -> file1    
    ```
