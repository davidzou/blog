rm
====

***

### 语法

```
rm [options] file ...
```

***

### 参数

* `-d` 强制删除无其他类型文件的空目录。效果同rmdir。
* `-f` 在不提示确认的情况下强制删除文件，而不管文件的权限如何。如果文件不存在，则不要显示诊断消息或修改退出状态以反映错误。-f选项覆盖以前的任何-i选项。
* `-i` 移除文件之前提示用户进行确认。
* `-P` 在删除常规文件之前覆盖它们。文件被覆盖三次，首先使用字节模式0xff，然后0x00，然后再次0xff，然后再删除它们。
* `-r` 如果file是一个目录时，则递归地移除整个子目录树。
* `-R` 同 `-r`
* `-v` 显示删除文件的信息
* `-W` 尝试撤消删除命名文件。目前，此选项只能用于恢复由whiteout生成的文件。（此参数未遇到过使用场景，如有请告知。）

***

### Example

* `rm -d src`

    删除目录，如果目录为空目录则删除成功。也就是rmdir的操作

    ```
    zzw:temp zzw$ rm -d src
    rm: src: Directory not empty
    zzw:temp zzw$ rmdir src
    rmdir: src: Directory not empty
    ```

* `rm -f file1 file2 file3 file4 file5 file6 file7`

    删除文件所有文件，-f参数忽略所有文件权限直接删除，且无提示。

    ```
    zzw:src3 zzw$ ls -al
    total 0
    drwxr-xr-x  9 zzw  staff  288  3 12 02:00 .
    drwxr-xr-x  6 zzw  staff  192  3 12 01:56 ..
    -rwxr-xr-x  1 zzw  staff    0  3 12 01:56 file1
    -rwxr--r--  1 zzw  staff    0  3 12 01:56 file2
    -rwxr--r--  1 zzw  staff    0  3 12 01:56 file3
    -rwx------  1 zzw  staff    0  3 12 01:58 file4
    -rw-------  1 zzw  staff    0  3 12 01:59 file5
    -r--------  1 zzw  staff    0  3 12 01:59 file6
    ----------  1 zzw  staff    0  3 12 02:00 file7
    zzw:src3 zzw$ rm -f file1 file2 file3 file4 file5 file6 file7
    zzw:src3 zzw$ ls -al
    total 0
    drwxr-xr-x  2 zzw  staff   64  3 12 02:01 .
    drwxr-xr-x  6 zzw  staff  192  3 12 01:56 ..

    ```

* `rm -r src3`

    删除文件目录，并递归删除目录下的所有文件。

    ```
    zzw:temp zzw$ ls
    src  src1 src2 src3
    zzw:temp zzw$ rm -r src3
    zzw:temp zzw$ ls
    src  src1 src2
    ```

* `rm -P file1`

    很奇葩的一个操作，不知道为什么要这样。数据丢失后，同一个磁道被覆盖后会减小恢复的成功率。不知道和这个有关系嘛。

    ```
    zzw:temp zzw$ rm -P file1
    ```

* `rm -rv src2`

    看下-v的效果，这里递归删除了一个目录

    ```
    zzw:temp zzw$ rm -rv src2
    src2/file3
    src2/file4
    src2/file2
    src2/file1
    src2
    zzw:temp
    ```

* `rm -rf /`

    流传于世的神操作。当然你要有神权限对吧。
