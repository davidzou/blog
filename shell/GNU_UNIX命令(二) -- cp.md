cp
=====

***

### 语法

```
cp [options] file1 file2
cp [options] files directory
```

> 第一种形式可将file1复制到file2.如果file2已存在，而且你有足够的权限，那么file2将毫无预警的被覆盖（除非使用-i参数）。

> 第二种形式会将一个或者过个files文件复制到directory目录下。请注意，同时指定两个以上的参数时，cp它会假设最后一个参数为目的地，而且该目的地是目录，如果不存在，将会显示一个错误（通常为使用指南）

***

### 参数

* `-f` 强制覆盖现有的目标文件。
* `-i` 覆盖目标文件之前先提示用户进行确认。
* `-p` 保留文件的所有属性，包括拥有者、组、使用权限以及时间戳。
* `-r、-R` 递归赋值整个子目录树。
* `-v` 在复制之前显示文件名称。

***

### Example

* ```cp file1 file2```

    复制file1文件，以file2为副本

    ```
    zzw:temp zzw$ ls
    file1
    zzw:temp zzw$ cp file1 file2
    zzw:temp zzw$ ls
    file1 file2
    ```

* `cp -Rp src src2`

    复制src目录下的文件到另一个新的src2目录下, 并保留所有属性

    ```
    zzw:temp zzw$ ls
    file1 file2 src
    zzw:temp zzw$ ls -al src/
    total 16
    drwxr-xr-x  4 zzw  staff  128  3 10 00:25 .
    drwxr-xr-x  5 zzw  staff  160  3 10 00:24 ..
    -rwxrw-rw-  1 zzw  staff    6  3 10 00:25 file1
    -rw-r--r--  1 zzw  staff    6  3 10 00:25 file2
    zzw:temp zzw$ cp -Rp src src2
    zzw:temp zzw$ ls
    file1 file2 src   src2
    zzw:temp zzw$ ls -al src2
    total 16
    drwxr-xr-x  4 zzw  staff  128  3 10 00:25 .
    drwxr-xr-x  6 zzw  staff  192  3 10 00:29 ..
    -rwxrw-rw-  1 zzw  staff    6  3 10 00:25 file1
    -rw-r--r--  1 zzw  staff    6  3 10 00:25 file2
    ```

* `cp -R src src3`

    如果不保留属性的话，只要不加上-p的参数即可，如下file1的属性由原本的766变为了644

    ```
    zzw:temp zzw$ cp -R src src3
    zzw:temp zzw$ ls -al src3
    total 16
    drwxr-xr-x  4 zzw  staff  128  3 10 00:31 .
    drwxr-xr-x  7 zzw  staff  224  3 10 00:31 ..
    -rwxr--r--  1 zzw  staff    6  3 10 00:31 file1
    -rw-r--r--  1 zzw  staff    6  3 10 00:31 file2
    ```

* `cp file1 file2 file3 file4 allfiles`

    将当前目录下的file1、file2、file3、file4文件复制到allfiles目录下

    ```
    zzw:temp zzw$ mkdir allfiles
    zzw:temp zzw$ ls
    allfiles file1    file2    file3    file4    src      src2     src3
    zzw:temp zzw$ cp file1 file2 file3 file4 allfiles
    zzw:temp zzw$ ls -al allfiles/
    total 16
    drwxr-xr-x   6 zzw  staff  192  3 10 00:34 .
    drwxr-xr-x  10 zzw  staff  320  3 10 00:34 ..
    -rwxr--r--   1 zzw  staff    6  3 10 00:34 file1
    -rw-r--r--   1 zzw  staff    6  3 10 00:34 file2
    -rw-r--r--   1 zzw  staff    0  3 10 00:34 file3
    -rw-r--r--   1 zzw  staff    0  3 10 00:34 file4
    ```

    当然现实中有很多这种文件的话，一个个列举是很累的，那么就需要使用到通配符来解决了，省力有省事。

    ```
    zzw:temp zzw$ cp file* allfiles/
    zzw:temp zzw$ ls -al allfiles/
    total 16
    drwxr-xr-x   6 zzw  staff  192  3 10 00:36 .
    drwxr-xr-x  10 zzw  staff  320  3 10 00:34 ..
    -rwxr--r--   1 zzw  staff    6  3 10 00:36 file1
    -rw-r--r--   1 zzw  staff    6  3 10 00:36 file2
    -rw-r--r--   1 zzw  staff    0  3 10 00:36 file3
    -rw-r--r--   1 zzw  staff    0  3 10 00:36 file4
    ```

* `cp -v file1 file5`

    来看看-v的参数效果

    ```
    zzw:temp zzw$ cp -v file1 file5
    file1 -> file5
    ```

* `cp -vi file1 file5`

    来看看-i的参数效果，Linux环境下root账户已经设好了cp为cp -i的别名，这个操作习惯是很好的，建议设置以避免一些错误的操作。

    ```
    zzw:temp zzw$ cp -vi file1 file5
    overwrite file5? (y/n [n]) n
    not overwritten
    zzw:temp zzw$ cp -vi file1 file5
    overwrite file5? (y/n [n]) y
    file1 -> file5
    ```
