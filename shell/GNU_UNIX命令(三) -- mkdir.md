mkdir
====

***

### 语法

```
mkdir [options] directory_name
```

***

### 参数

* -m mode 将目录的权限模式设为mode。
* -p 创建目录时自动创建不存在的中间层目录。
* -v 创建目录时显示目录名称（-p同时存在时）

***

### Example

* `mkdir src`

    直接创建src目录，以及带上-v参数后的显示效果

    ```
    zzw:temp zzw$ mkdir src
    zzw:temp zzw$ ls -al
    drwxr-xr-x   2 zzw  staff   64  3 10 15:54 src
    ```

* `mkdir -v src1`

    ```
    zzw:temp zzw$ mkdir -v src1
    mkdir: created directory 'src1'
    zzw:temp zzw$ ls
    src  src1
    ```

* `mkdir -p src2/src3/src4`

    创建多级文件目录，中间目录都未曾创建过，使用-p同时创建。

    ```
    zzw:temp zzw$ mkdir -p src2/src3/src4
    zzw:temp zzw$ tree src2
    src2
    └── src3
        └── src4

    2 directories, 0 files
    ```

* `mkdir -m 666 src3`

    创建目录并将权限设置为666.

    ```
    zzw:temp zzw$ mkdir -m 666 src3
    zzw:temp zzw$ ls -al
    total 0
    drwxr-xr-x   6 zzw  staff  192  3 10 16:00 .
    drwxr-xr-x  31 zzw  staff  992  3 10 00:03 ..
    drwxr-xr-x   2 zzw  staff   64  3 10 15:54 src
    drwxr-xr-x   2 zzw  staff   64  3 10 15:54 src1
    drwxr-xr-x   3 zzw  staff   96  3 10 15:57 src2
    drw-rw-rw-   2 zzw  staff   64  3 10 16:00 src3
    ```
