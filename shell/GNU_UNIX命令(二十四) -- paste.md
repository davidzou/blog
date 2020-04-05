paste
====

### 语法

```
paste [options] files
```

将一个或多个文件的内容横向排列在一起。

***

### 参数

* `-d | --delimiters=LIST` 以LIST为字段的分割字符。（默认是tab）

* `-s | --serial`   将输入文件的内容纵向排列在一起。

* `-z | --zero-terminated` 所有换行符看做为NUL，不作为新行。

* `--help` 显示帮助

* `--version`  显示版本。

***

### Example

* `paste file1 file2`

    一般合并。

    ```
    root@3b4e07cd9fdd:/# paste file1 file2
    line1	number1
    line2	number2
    line3	number3
    	    number4
    line5	number5

    line7	number7
    root@3b4e07cd9fdd:/# cat file1
    line1
    line2
    line3

    line5

    line7
    root@3b4e07cd9fdd:/# cat file2
    number1
    number2
    number3
    number4
    number5

    number7

    ```

* `paste -s file1 file2`

    纵向排列。

    ```
    root@3b4e07cd9fdd:/# paste -s file1 file2
    line1	line2	line3		line5		line7
    number1	number2	number3	number4	number5		number7

    ```

* `paste -d '@' file1 file2`

    指定分隔符`@`, 但是看到第六行空行也显示在内。

    ```
    root@3b4e07cd9fdd:/# paste -d '@' file1 file2
    line1@number1
    line2@number2
    line3@number3
    @number4
    line5@number5
    @
    line7@number7
    ```

* `paste -d '@' -z file1 file2`

    `-z`表示没有换行符作为一行数据，此时看到就还有一个`@`分割。

    ```
    root@3b4e07cd9fdd:/# paste -d '@' -z file1 file2
    line1
    line2
    line3

    line5

    line7
    @number1
    number2
    number3
    number4
    number5

    number7
    ```

* `paste --version`

    看下版本。

    ```
    root@3b4e07cd9fdd:/# paste --version
    paste (GNU coreutils) 8.28
    Copyright (C) 2017 Free Software Foundation, Inc.
    License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>.
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.

    Written by David M. Ihnat and David MacKenzie.
    ```
