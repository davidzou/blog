unexpand
=====

***

### 语法

```
unexpand [options] file
```

> 看名字就知道和`expand`是相反效果了。

***

### 参数

* `-a | --all`  转换所有空白。

* `--first-only`    仅转换空白的前导序列（重写-a）。

* `-t | --tabs=N`   将制表符N个字符分开，而不是8个字符（启用-a）

* `-t | --tabs=LIST`

    使用逗号分隔的制表位列表最后一个指定的位置可以加上前缀“/”以指定要在最后一个显式指定的制表位结束后使用的制表位大小。也可以使用前缀“+”将剩余的制表位相对于最后指定的制表位而不是第一列对齐。

* `--help`  显示帮助信息。

* `--version`   显示版本信息。

***

### Example

* `unexpand -a file`

    ``` 
    root@ecd79fccf553:/# cat file
    line1 with a space.
    line2 .
    line3.
    with a tab	line4.
    with four space    line5.
    with eight space        line6.
    root@ecd79fccf553:/# unexpand -a file
    line1 with a space.
    line2 .
    line3.
    with a tab	line4.
    with four space	   line5.
    with eight space	    line6.
    ```
  
 * `unexpand -a -t 4 file`
 
    ```
    root@ecd79fccf553:/# cat file
    line1 with a space.
    line2 .
    line3.
    with a tab	line4.
    with four space    line5.
    with eight space        line6.
    root@ecd79fccf553:/# unexpand -a -t 4 file
    line1 with a space.
    line2 .
    line3.
    with a tab	line4.
    with four space	   line5.
    with eight space                line6. 
    ```
