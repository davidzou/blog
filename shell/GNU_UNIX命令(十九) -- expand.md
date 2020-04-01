expand
====

### 语法

`expand [options] files`

将文件中的tab字符转换成等宽的若干个space字符。虽然tab有助于排版，但不同编辑器的显示还是存在一定差别。多数IDE中tab的表现标准源头就在这里了。

***

### 参数

* `-t | --tab=NUM` 指定tab字符的等效空格宽度（默认值为8）。

* `-i | -- initial` 只转换出现在行首的tab字符。

* `--help` 显示帮助文档

* `--version` 显示当前文本。

***

### Example

* `expand test.txt`

    标准替换为8个space。

    ```
    |zzw:~ zzw$ cat test.txt
    |		front with 2 tabs.
    |    front with 4 spaces.
    |  font with 2 spaces.
    |no tab and space.
    |	font with 1 tab.
    |  |				font with 2 space and 4 tabs.
    |zzw:~ zzw$ expand test.txt
    |                front with 2 tabs.
    |    front with 4 spaces.
    |  font with 2 spaces.
    |no tab and space.
    |        font with 1 tab.
    |  |                             font with 2 space and 4 tabs.
    ```

* `expand -t 1 test.txt`

    将tab替换为1个space的效果。

    ```
    |zzw:~ zzw$ cat test.txt
    |		front with 2 tabs.
    |    front with 4 spaces.
    |  font with 2 spaces.
    |no tab and space.
    |	font with 1 tab.
    |  |				font with 2 space and 4 tabs.
    |zzw:~ zzw$ expand -t 1 test.txt
    |  front with 2 tabs.
    |    front with 4 spaces.
    |  font with 2 spaces.
    |no tab and space.
    | font with 1 tab.
    |  |    font with 2 space and 4 tabs.
    ```

* `expand --version`

    ```
    root@895e4cf6e8a1:/# expand --version
    expand (GNU coreutils) 8.28
    Copyright (C) 2017 Free Software Foundation, Inc.
    License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>.
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.

    Written by David MacKenzie.
    ```
