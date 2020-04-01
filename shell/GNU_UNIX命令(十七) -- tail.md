tail
====

### 语法

`tail [options] files`

***

### 参数

* `-b NUM` 显示`NUM`个块内容，每个块的大小为512byte。（最新的[coreutils](https://www.gnu.org/software/coreutils/)中并未包含）

* `-c | --byte=[+]NUM` 显示文件中最后`NUM`个字节的内容。

* `-f | --follow=[name|descriptor]` 显示文件尾端内容后不关闭输入文件，并且在输入文件的末端出现新内容时实时将它们显示出来。通常用此选项来持续观察日志文件的内容变化情况。

* `-F` 和`--follow=name --retry`效果一致。

* `-n NUM` 显示出最后`NUM`行的内容，默认值是10.

* `-q | --quiet | --silent` 任何情况下不显示文件名。

* `--retry` 如果无法访问，会继续尝试打开文件。

* `-s | --sleep-interval=N` 配合`-f`使用，在数据刷新之间睡眠大约N秒（默认为1.0；

* `-v | --verbose` 显示文件名信息。

* `-z | --zero-terminated` 将所有换行符看做为NUL，不作为新行。

* `--help` 显示帮助信息。

* `--version` 显示命令版本。

***

### Example

* `tail test`

    显示文件默认末尾10行。

    ```
    root@3ff83ffc7119:/# tail test
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

* `tail -n 5 test`

    显示文件指定5行的末尾内容。

    ```
    root@3ff83ffc7119:/# tail -n 5 test
    line3 copy
    This is line4 copy
    This is line5 copy
    This is line6 copy
    This is line7 copy
    ```

* `tail -c 10 test`

    显示末尾10个字节的内容。

    ```
    root@3ff83ffc7119:/# tail -c 10 test
    ine7 copy
    ```

* `tail -n 5 -v test`

    显示文件名，`==> test <==`中的test就是文件名。

    ```
    root@3ff83ffc7119:/# tail -n 5 -v test
    ==> test <==
    line3 copy
    This is line4 copy
    This is line5 copy
    This is line6 copy
    This is line7 copy
    ```

* `tail --version`

    看下版本信息。

    ```
    root@3ff83ffc7119:/# tail --version
    tail (GNU coreutils) 8.28
    Copyright (C) 2017 Free Software Foundation, Inc.
    License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>.
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.

    Written by Paul Rubin, David MacKenzie, Ian Lance Taylor,
    and Jim Meyering.
    ```
