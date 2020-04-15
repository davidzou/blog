tr
=====

***

### 语法

```
tr [options] SET1 [SET2]
```

***

### 参数

* `-c | -C | --complement`  使用SET1的补码。

* `-d | --delete`  删除SET1中的字符。

* `-s | --squeeze-repeats`  将最后一个指定集中列出的重复字符的每个序列替换为该字符的单个出现

* `-t | --truncate-set1`    首先将SET1截断为SET2的长度

* `--help`  显示帮助信息。

* `--version`   显示版本信息。

字符集的范围：

- `\NNN` 八进制值的字符 NNN
- `\\` 反斜杠
- `\a` Ctrl-G 铃声
- `\b` Ctrl-H 退格符
- `\f` Ctrl-L 走行换页
- `\n` Ctrl-J 新行
- `\r` Ctrl-M 回车
- `\t` Ctrl-I tab键
- `\v` Ctrl-X 水平制表符
- `CHAR1-CHAR2` 字符范围从 CHAR1 到 CHAR2 的指定，范围的指定以 ASCII 码的次序为基础，只能由小到大，不能由大到小。
- `[CHAR*]` 这是 SET2 专用的设定，功能是重复指定的字符到与 SET1 相同长度为止
- `[CHAR*REPEAT]`   这也是 SET2 专用的设定，功能是重复指定的字符到设定的 REPEAT 次数为止(REPEAT 的数字采 8 进位制计算，以 0 为开始)
- `[:alnum:]`   所有字母字符与数字
- `[:alpha:]`   所有字母字符
- `[:blank:]`   所有水平空格
- `[:cntrl:]`   所有控制字符
- `[:digit:]`   所有数字
- `[:graph:]`   所有可打印的字符(不包含空格符)
- `[:lower:]`   所有小写字母
- `[:print:]`   所有可打印的字符(包含空格符)
- `[:punct:]`   所有标点字符
- `[:space:]`   所有水平与垂直空格符
- `[:upper:]`   所有大写字母
- `[:xdigit:]`  所有 16 进位制的数字
- `[=CHAR=]`    所有符合指定的字符(等号里的 CHAR，代表你可自订的字符)

***

### Example

* `tr a-z A-Z`

    转化大写。

    ```
    root@ecd79fccf553:/# cat file | tr a-z A-Z
    LINE1 WITH A SPACE.
    LINE2 .
    LINE3.
    WITH A TAB	LINE4.
    WITH FOUR SPACE    LINE5.
    WITH EIGHT SPACE        LINE6.
    root@ecd79fccf553:/# cat file
    line1 with a space.
    line2 .
    line3.
    with a tab	line4.
    with four space    line5.
    with eight space        line6. 
    ```
  
 * `tr [:blank:] 7`
 
    将空格替换为`7`。
 
    ```
    root@ecd79fccf553:/# cat file | tr [:blank:] 7
    line17with7a7space.
    line27.
    line3.
    with7a7tab7line4.
    with7four7space7777line5.
    with7eight7space77777777line6. 
    ```
