cut
=====

***

### 语法

```
cut [options] file
```

***

### 参数

* `-b | --byte=LIST`    仅选择`LIST`范围内给定的这些字节。

* `-c | --characters=LIST`  仅选择`LIST`范围内给定的这些字符。

* `-d | --delimiter=DELIM`  使用`DELIM`替换TAB作为字段分隔符。

* `-f | --field=LIST` 仅选择这些字段；也打印不包含分隔符的任何行，除非指定了-s选项

* `-n` 忽略。

* `--complement` 对选定的字节、字符或字段集进行补足。

* `-s | --only-delimited`   不打印不包含分隔符的行。

* `--output-delimiter=STRING`   使用字符串`STRING`作为输出分隔符, 默认为使用输入分隔符

* `-z | --zero-terminated`  将所有换行符看做为NUL，不作为新行。

* `--help`  显示帮助信息。

* `--version`   显示版本信息。


__LIST范围表示__

- `N`   第N个字节、字符或字段，从1开始计数

- `N-`  从第N字节、字符或字段到行尾

- `N-M` 从N到M（包括）字节、字符或字段

- `-M`  从第一个到第M个（包括）字节、字符或字段

***

### Example

* `cut -b 2`

    截取指定范围。

    ```
    root@5ca8dbe55d8d:/# echo "hello world" | cut -b 2
    e
    root@5ca8dbe55d8d:/# echo "hello world" | cut -b 2-
    ello world
    root@5ca8dbe55d8d:/# echo "hello world" | cut -b 2-8
    ello wo
    root@5ca8dbe55d8d:/# echo "hello world" | cut -b -8
    hello wo
    ```

* `cut -b 1 --complement`

    补足。

    ```
    root@5ca8dbe55d8d:/# echo "hello world!" | cut -b 1 --complement
    ello world!
    ```

* `cut -d: -f1 /etc/passwd`

    显示`/etc/passwd`文件中的第一个字段，即用户名称。

    ```
    root@5ca8dbe55d8d:/# cut -d: -f1 /etc/passwd
    root
    daemon
    bin
    sys
    sync
    games
    man
    lp
    mail
    news
    uucp
    proxy
    www-data
    backup
    list
    irc
    gnats
    nobody
    _apt
    ```

* `cut -c 1 /etc/passwd`

    显示`/etc/passwd`文件每行的第一个字段。

    ```
    root@5ca8dbe55d8d:/# cut -c 1 /etc/passwd
    r
    d
    b
    s
    s
    g
    m
    l
    m
    n
    u
    p
    w
    b
    l
    i
    g
    n
    _

    ```

* `cut -s -d i -f1 file1`

    不显示不包含分隔符为`i`的行，这里即空行不显示。

    ```
    root@5ca8dbe55d8d:/# cut -s -d i -f2- file1
    ne1
    ne2
    ne3
    ne4
    ne5
    root@5ca8dbe55d8d:/# cat file1
    line1
    line2
    line3

    line4
    line5
    ```
