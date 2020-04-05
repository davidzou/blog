od
====

### 语法

```
od [options] files
```

以八进制数值格式或者其他指定方式显示文件内容。

***

### 参数

* `-t | --format=TYPE` 选择字节的表示格式。

* `--help` 显示帮助。

* `--version` 显示版本。


__各TYPE格式如下：__

    * `a`   字符名称

    * `o1`  八进制数值

    * `c`   ASCII或backslash escape

    * `u2`  无符号十进制整形。

    * `fF`  浮点数

    * `dI`  十进制整数

    * `dL`  十进制长整形。

    * `o2`  双字节的八进制数值。

    * `d2`  双字节十进制数值

    * `x2`  十六进制数值。

***

### Example

* `od test.txt`

    默认表示

    ```
    root@3b4e07cd9fdd:/# od test.txt
    0000000 064554 062556 005061 064554 062556 005062 005012 030060
    0000020 005012 064554 062556 005063 064554 062556 005064
    0000036
    root@3b4e07cd9fdd:/# cat test.txt
    line1
    line2


    00

    line3
    line4
    ```

* `od -t a test.txt`

    字符名称表示。

    ```
    root@3b4e07cd9fdd:/# od -t a test.txt
    0000000   l   i   n   e   1  nl   l   i   n   e   2  nl  nl  nl   0   0
    0000020  nl  nl   l   i   n   e   3  nl   l   i   n   e   4  nl
    0000036
    ```

* `od -t c test.txt`

    ASCII表示。

    ```
    root@3b4e07cd9fdd:/# od -t c test.txt
    0000000   l   i   n   e   1  \n   l   i   n   e   2  \n  \n  \n   0   0
    0000020  \n  \n   l   i   n   e   3  \n   l   i   n   e   4  \n
    0000036
    ```

* `od -t o test.txt `

    八进制表示。

    ```
    root@3b4e07cd9fdd:/# od -t o test.txt
    0000000 14533464554 15133005061 01214462556 06014005012
    0000020 15133005012 01214662556 14533464554 00000005064
    0000036
    ```

* `od -t x test.txt`

    十六进制表示。

    ```
    root@3b4e07cd9fdd:/# od -t x test.txt
    0000000 656e696c 696c0a31 0a32656e 30300a0a
    0000020 696c0a0a 0a33656e 656e696c 00000a34
    0000036
    ```
