tac
=====

***

### 语法

```
tac [options] file
```

正好是`cat`反过来，很有意思的一条命令。

***

### 参数

`-b | --before` 在之前而不是之后连接分隔符。

`-r | --regex`  将分隔符解释为正则表达式。

`-s --separator=STRING` 使用`STRING`作为分隔符而不是换行符。

`--help` 显示帮助信息。

`--version` 显示版本信息。

***

### Example

* `tac file1`

    颠倒输入行的顺序，和`cat`效果正好颠倒。

    ```
    root@5ca8dbe55d8d:/# tac file1
    line5
    line4

    line3
    line2
    line1
    ```

* `tac -b file2`

    合并了最后两行，但是明显看到有两行空行，具体使用场景也很难想到。

    ```
    root@5ca8dbe55d8d:/# tac -b file2


    three
    twooneroot@5ca8dbe55d8d:/#
    ```

* `tac --version`

    看下版本，不是所有的系统都包含这个命令，MAC下就没有。这里看到的命令版本都是源于GNU coreutils这个库。

    ```
    root@5ca8dbe55d8d:/# tac --version
    tac (GNU coreutils) 8.28
    Copyright (C) 2017 Free Software Foundation, Inc.
    License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>.
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.

    Written by Jay Lepreau and David MacKenzie.
    ```
