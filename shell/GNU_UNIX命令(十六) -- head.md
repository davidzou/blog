head
====

### 语法

`head [options] files`

默认打印每个文档首10行内容。

***

### 参数

* `-c | --bytes=[-]NUM` 显示前`NUM`个字节的内容。后面如添加了计数单位将会相应改变。`-NUM`表示出去末尾的个数字节。

* `-n | --lines=[-]NUM` 显示前`NUM`行。默认值为10。

* `-q | --quiet | --silent` 不打印头部显示的文件名。

* `-v  | --verbose` 打印头部显示的文件名。

* `-z | --zero-terminated` 将所有换行符看做为NUL，不作为新行。

* `--help` 显示帮助文档。

* `--version` 显示版本信息。

***

### Example

* `head test`

    默认显示文档前10行内容。

    ```
    root@3ff83ffc7119:/# head test
    line1
    line2
    line3
    This is line4
    This is line5
    This is line6
    This is line7

    line1 copy
    line2 copy
    ```

* `head -n 5 test`

    指定显示行数为开头5行。

    ```
    root@3ff83ffc7119:/# head -n 5 test
    line1
    line2
    line3
    This is line4
    This is line5
    ```

* `head -c 5 test`

    显示指定字节数，单位计量表示`b 512, kB 1000, K 1024, MB 1000*1000, M 1024*1024, GB 1000*1000*1000, G 1024*1024*1024,  T, P, E, Z, Y后续亦如此规律。`

    ```
    root@3ff83ffc7119:/# head -c 5 test
    line1root@3ff83ffc7119:/#
    root@3ff83ffc7119:/# head -c 6 test
    line1 root@3ff83ffc7119:/# head -c 6k test
    line1
    line2
    line3
    This is line4
    This is line5
    This is line6
    This is line7

    line1 copy
    line2 copy
    line3 copy
    This is line4 copy
    This is line5 copy
    This is line6 copy
    This is line7 copy
    ```

* `head -v test`

    显示更多的显示信息，`==> test <==` 这里表示文件名

    ```
    root@3ff83ffc7119:/# head -v -n 5 test
    ==> test <==
    line1
    line2
    line3
    This is line4
    This is line5
    root@3ff83ffc7119:/# head -v test
    ==> test <==
    line1
    line2
    line3
    This is line4
    This is line5
    This is line6
    This is line7

    line1 copy
    line2 copy
    ```

* `head -z test`

    不计算换行。

    ```
    root@3ff83ffc7119:/# head -z test
    line1
    line2
    line3
    This is line4
    This is line5
    This is line6
    This is line7

    line1 copy
    line2 copy
    line3 copy
    This is line4 copy
    This is line5 copy
    This is line6 copy
    This is line7 copy
    ```

* `head --version`

    显示当前版本

    ```
    root@3ff83ffc7119:/# head --version
    head (GNU coreutils) 8.28
    Copyright (C) 2017 Free Software Foundation, Inc.
    License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>.
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.

    Written by David MacKenzie and Jim Meyering.
    ```
