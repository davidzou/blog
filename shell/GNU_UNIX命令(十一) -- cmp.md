cmp
====

***

### 语法

```
cmp [options] file1 file2
```

> 比较2个文件，一个字节一个字节的比较。很酷吧！

***

### 参数

* `-b  --print-bytes` 显示不同的字节。

* `-i SKIP  --ignore-initial=SKIP` 跳过第一个不同字符。

* `-i SKIP1:SKIP2  --ignore-initial=SKIP1:SKIP2` 跳过FILE1的第一个SKIP1字节和FILE2的第一个SKIP2字节。

    ###### SKIP1和SKIP2是每个文件中要跳过的字节数。跳过值后面可以跟下列乘法后缀：kB 1000，K 1024，MB 1000000，M 1048576，GB 100000000，G 1073741824，依此类推T、P、E、Z、Y。


* `-l  --verbose` 显示所有不同字节的字节数和值。

* `-n LIMIT  --bytes=LIMIT`  比较限定范围内的字符。

* `-s  --quiet  --silent` 无输出；仅输出退出状态。

* `-v  --version` 显示版本信息。

* `--help` 显示帮助信息。

***

### Example

* `cmp file1 file2`

    只是比较2个文件，不同处在第一行的第五个字符。

    ```
    zzw:temp zzw$ cmp file1 file2
    file1 file2 differ: char 5, line 1
    zzw:temp zzw$ cat file1
    file1
    zzw:temp zzw$ cat file2
    file2
    ```

* `cmp -b file1 file2`

    输出不同的内容，1和2不同。

    ```
    zzw:temp zzw$ cmp -b file1 file2
    file1 file2 differ: byte 5, line 1 is  61 1  62 2
    zzw:temp zzw$ cat file1
    file1
    zzw:temp zzw$ cat file2
    file2
    ```

* `cmp -i 5 -b file1 file2`

    跳过第一个不同的地方在第5的字节处。

    ```
    zzw:temp zzw$ cmp -i 5 -b file1 file2
    file1 file2 differ: byte 2, line 2 is 141 a 101 A
    zzw:temp zzw$ cat file1
    file1
    a
    zzw:temp zzw$ cat file2
    file2
    A

    ```

* `cmp -l file1 file2`

    检查所有的不同处，`cmp: EOF on file1` 表示file1结束了，但file2还没有结束后面的都不一样了。

    ```
    zzw:temp zzw$ cmp -l file1 file2
    5  61  62
    cmp: EOF on file1
    zzw:temp zzw$ cat file1
    file1
    zzw:temp zzw$ cat file2
    file2
    A
    ```

* `cmp -n 6 -l file1 file2`

    最大可比较的字符数。

    ```
    zzw:temp zzw$ cmp -n 6 -l file1 file2
    5  61  62
    zzw:temp zzw$ cmp -n 5 -l file1 file2
    5  61  62
    zzw:temp zzw$ cmp -n 4 -l file1 file2
    zzw:temp zzw$ cat file1
    file1
    a
    zzw:temp zzw$ cat file2
    file2
    A
    ```
