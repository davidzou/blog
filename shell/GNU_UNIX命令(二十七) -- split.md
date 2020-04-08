split
=====

***

### 语法

```
split [options] infile [outfile]
```

分割大文件成若干个小文件，这对于数据分析来说很有用。

***

### 参数

* `-a | --suffinx-length=N`    生成长度为N的后缀（默认值2）。

* `--additional-suffix=SUFFIX`   在文件名后面附加一个后缀。

* `-b | --bytes=SIZE`   按指定的字节数量分割文件。

* `-C | --line-bytes=SIZE` 按行为单位，能放入最多的字节数分割。

* `-d`  使用从0开始的数字后缀，而不是字母后缀。

* `--numeric-suffixes[=FROM]`   与-d相同，但允许设置起始值。

* `-x`  使用从0开始的十六进制后缀，而不是按字母顺序。

* `--hex-suffixes[=FROM]`   与-x相同，但允许设置起始值。

* `-e`  不生成带有'-n'的空输出文件。

* `--filter=COMMAND`

* `-l | --lines=NUMBER` 为每个输出文件放置编号行/记录。

* `-n | --number=CHUNKS`    按块的方式分割。

* `-t | --separator=SEP`    使用`SEP`而不是newline作为记录分隔符；`\0`代表NUL字符。

* `-u | --unbuffered`   立即用'-n r/..'将输入复制到输出。

* `--verbose`   显示操作详细信息。


***

### Example

* `split -5 -a 5 file1 file1split`

    分割file1内容，5行一个文件，后缀名标注为5位即`-aaaaa`和`-aaaab`两个后缀。

    ```
    root@5ca8dbe55d8d:/# split -5 -a 5 file1 file1split
    root@5ca8dbe55d8d:/# ls
    file1  file1splitaaaaa  file1splitaaaab
    root@5ca8dbe55d8d:/# cat file1splitaaaaa
    line1
    line2
    line3

    line4
    root@5ca8dbe55d8d:/# cat file1splitaaaab
    line5
    ```

* `split -5 --additional-suffix=awsome file1 file1split`

    添加文件名后缀。

    ```
    root@5ca8dbe55d8d:/test# split -5 --additional-suffix=awsome file1 file1split
    root@5ca8dbe55d8d:/test# ls
    file1  file1splitaaawsome  file1splitabawsome
    ```

* `split -b 10 file1`

    每10个字节分割文件。

    ```
    root@5ca8dbe55d8d:/test# split -b 10 file1
    root@5ca8dbe55d8d:/test# ls
    file1  xaa  xab  xac  xad
    root@5ca8dbe55d8d:/test# cat xaa
    line1
    lineroot
    ```

* `split -C 10 file1`

    按行分割。

    ```
    root@5ca8dbe55d8d:/test# split -C 10 file1
    root@5ca8dbe55d8d:/test# ls
    file1  xaa  xab  xac  xad  xae
    root@5ca8dbe55d8d:/test# cat xaa
    line1
    ```

* `split -5 -d file1 file1split`

    `-d`使用数字命名。

    ```
    root@5ca8dbe55d8d:/test# split -5 -d file1 file1split
    root@5ca8dbe55d8d:/test# ls
    file1  file1split00  file1split01
    root@5ca8dbe55d8d:/test# split -5 -d --numeric-suffixes=10 file1 file1split
    root@5ca8dbe55d8d:/test# ls
    file1  file1split10  file1split11
    ```

* `split -b 2 -x file1 file1split`

    `-x`分割后十六进制命名。

    ```
    root@5ca8dbe55d8d:/test# split -b 2 -x file1 file1split
    root@5ca8dbe55d8d:/test# ls
    file1         file1split01  file1split03  file1split05  file1split07  file1split09  file1split0b  file1split0d  file1split0f
    file1split00  file1split02  file1split04  file1split06  file1split08  file1split0a  file1split0c  file1split0e
    root@5ca8dbe55d8d:/test# split -b 2 -x --hex-suffixes=10 file1 file1split
    root@5ca8dbe55d8d:/test# ls
    file1         file1split11  file1split13  file1split15  file1split17  file1split19  file1split1b  file1split1d  file1split1f
    file1split10  file1split12  file1split14  file1split16  file1split18  file1split1a  file1split1c  file1split1e
    ```

* `split -n 3 file1`

    将文分成3块。

    ```
    root@5ca8dbe55d8d:/test# split -n 3 file1
    root@5ca8dbe55d8d:/test# ls
    file1  xaa  xab  xac
    ```

* `split -5 --verbose file1`

    分割信息。
    
    ```
    root@5ca8dbe55d8d:/test# split -5 --verbose file1
    creating file 'xaa'
    creating file 'xab'
    ```
