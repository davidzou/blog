mv
====

***

### 语法

```
mv [options] source target
mv [options] source ... directory
```

>在第一种形式中target为文件名时，命令mv就是rename的操作。

>在第二种形式中，mv将源操作数个文件移动到目标文件目录下。目标路径是由命名文件的最后一个操作符、一个斜杠或者一个路径名连接而产生的路径名。也就是说不是一个连续路径的时候必须使用‘/’来表示文件目录类型。

***

### 参数

* -f 强制完成移动，如果target存在，则会没有警告信息。
* -i 移动文件之前提示用户进行确认。
* -n 不要覆盖现有文件。（n选项将覆盖以前的任何-f或-i选项。）
* -v 移动时显示详情。

***

### Example

* `mv file1 src/`

    将file1文件移动到src目录下

    ```
    zzw:temp zzw$ mv file1 src/
    zzw:temp zzw$ tree src/
    src/
    └── file1

    0 directories, 1 file
    ```

* `mv file2 src1/`

    当移动目录不存在时，将提示错误信息，src1后面的'/'不添加时，文件夹不存在那么就代表重命名文件

    ```
    zzw:temp zzw$ ls
    file2 src
    zzw:temp zzw$ mv file2 src1/
    mv: rename file2 to src1/: No such file or directory
    zzw:temp zzw$ mv file2 src1
    zzw:temp zzw$ ls -al
    total 8
    drwxr-xr-x   4 zzw  staff  128  3 11 21:57 .
    drwxr-xr-x  31 zzw  staff  992  3 10 00:03 ..
    drwxr-xr-x   3 zzw  staff   96  3 11 21:53 src
    -rw-------   1 zzw  staff   12  3 11 10:58 src1
    ```

* `mv -n file1 file2`

    使用参数-n是不会覆盖现有已存在的文件。

    ```
    zzw:temp zzw$ ls
    file1 file2 src
    zzw:temp zzw$ cat file1
    hello file1
    zzw:temp zzw$ cat file2
    hello file2
    zzw:temp zzw$ mv -n file1 file2
    zzw:temp zzw$ cat file2
    hello file2
    ```

* `mv -i file1 file2`

    使用参数-i时会有提示信息显示。如果文件存在会覆盖原有文件除非使用-n参数。

    ```
    zzw:temp zzw$ mv -i file1 file2
    overwrite file2? (y/n [n]) n
    not overwritten
    zzw:temp zzw$ mv -i file1 file2
    overwrite file2? (y/n [n]) y
    zzw:temp zzw$ cat file2
    hello file1
    ```
* `mv -v file2 src/`

    看下-v参数的显示效果

    ```
    zzw:temp zzw$ mv -v file2 src/
    file2 -> src/file2
    ```
