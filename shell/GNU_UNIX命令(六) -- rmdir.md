rmdir
====

***

### 语法

```
rmdir [options] directories
```

> 删除空的目录，仅此而已，这是一个很少会用到的命令，最早出现在AT&T UNIX中。现在你可以用rm -d 代替。

***

### 参数

* `-p`  连同中间层目录一并删除。此选项可以用来移除子目录树。

***

### Example

* `rmdir src`

    可以看到这里src不是一个空目录不能被删除。rmdir只能删除空目录。

    ```
    zzw:temp zzw$ rm -d src
    rm: src: Directory not empty
    zzw:temp zzw$ rmdir src
    rmdir: src: Directory not empty
    ```

* `rmdir -p src/src1/src2/src3/src4`

    一并删除所有目录，可以看到删除第一级目录是没有效果了，必须是全路径。

    ```
    zzw:temp zzw$ mkdir -p src/src1/src2/src3/src4
    zzw:temp zzw$ tree src
    src
    └── src1
        └── src2
            └── src3
                └── src4

    4 directories, 0 files
    zzw:temp zzw$ rmdir -p src
    rmdir: src: Directory not empty
    zzw:temp zzw$ rmdir -p src/src1/src2/src3/src4
    zzw:temp zzw$ tree
    .

    0 directories, 0 files
    ```
