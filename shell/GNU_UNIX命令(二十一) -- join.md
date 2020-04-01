join
====

### 语法

`join [options] file1 file2`

顾名思义，依据指定的条件将来自file1和file2的两个表格合并为一个。有点sql join的感觉。

***

### 参数

* `-a FILENUM` 除了显示原来的输出内容之外，还显示指令文件中没有相同栏位的行。FILENUM值仅为`1`或`2`，分别代表文件`file1`和`file2`.

* `-e EMPTY` 若[文件1]与[文件2]中找不到指定的栏位，则在输出中填入选项中的字符串。

* `-i | --igore-case` 比较栏位内容时，忽略大小写的差异。

* `-j FIELD` 相当于参数`-1 FILED -2 FIELD`。

* `-o FORMAT` 按照指定的格式来显示结果。

* `-t CHAR` 使用栏位的分隔字符。

* `-v FILENUM` 跟`-a`相同，但是只显示文件中没有相同栏位的行。

* `-1 FILED` 连接`file1`指定的栏位。

* `-2 FILED` 连接`file2`指定的栏位。

* `--check-order` 检查输入是否正确排序，即使所有输入行都是可配对的。

* `--nocheck-order` 不要检查输入是否正确排序。

* `--header` 将每个文件中的第一行视为字段标题，打印它们而不尝试将它们配对

* `-z, --zero-terminated` 将所有换行符看做为NUL，不作为新行。

* `--help` 显示帮助。

* `--version` 显示版本信息。

***

### Example

* `join file1 file2`

    ```
    root@84d0e3c8d5ea:/# join file1 file2
    1. one 1
    2. two 2
    3. three 3
    root@84d0e3c8d5ea:/# cat file1
    1. one
    2. two
    3. three

    root@84d0e3c8d5ea:/# cat file2
    1. 1
    2. 2
    3. 3
    ```

* `join -a file1 file2`

    显示不同的内容。

    ```
    root@84d0e3c8d5ea:/# join -a 1 file1 file2
    1. one 1
    2. two 2
    3. three 3
    // 这里不同
    root@84d0e3c8d5ea:/# cat file1
    1. one
    2. two
    3. three
    // 这里是空行
    root@84d0e3c8d5ea:/# cat file2
    1. 1
    2. 2
    3. 3
    ```

* `join -a 1 -e HH file1 file2`

    不相同的用指定的字符串替换。

    ```
    root@84d0e3c8d5ea:/# join -a 1 -e HH file1 file2
    1. one 1
    2. two 2
    3. three 3
    HH
    ```

* `join -1 1 -2 1 file1 file2`

    合并`file1`第一字段，`file2`第一字段。

    ```
    root@84d0e3c8d5ea:/# join -1 1 -2 1 file1 file2
    1. one 1
    2. two 2
    3. three 3
    ```

* `join -j 1 file1 file2`

    简化`-1`和`-2`的。

    ```
    root@84d0e3c8d5ea:/# join -j 1 file1 file2
    1. one 1
    2. two 2
    3. three 3
    root@84d0e3c8d5ea:/# join -j 2 file1 file2 // 这里没有第二列数据
    join: file1:3: is not sorted: 3. three
    ```

* `join --nocheck-order -1 2 -2 1 file1 file2`

    无任何错误提示。也无输出。

    ```
    root@84d0e3c8d5ea:/# join --nocheck-order -1 2 -2 1 file1 file2
    ```

* `join -t '.' file1 file2`

    使用指定字符分割。

    ```
    root@84d0e3c8d5ea:/# join -t '.' file1 file2
    1. one what. 1
    2. two how. 2
    3. three why. 3
    ```

* `join -t '.' -o 1.1 1.2 1.3 1.4 2.1 file1 file2`

    格式化分割。这里的格式是`FILENUM.FIELD`。

    ```
    root@84d0e3c8d5ea:/# join -t '.' -o 1.1 1.2 1.3 1.4 2.1 file1 file2
    1. one what...1
    2. two how...2
    3. three why...3
    ```

* `join --version`

    看下版本。

    ```
    root@84d0e3c8d5ea:/# join --version
    join (GNU coreutils) 8.28
    Copyright (C) 2017 Free Software Foundation, Inc.
    License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>.
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.

    Written by Mike Haertel.
    ```
