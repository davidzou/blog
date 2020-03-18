file
====

***

### 语法

```
file [options] file
file [--help]
```

> file 用于查看文件类型，Ubuntu已经不自带了，需要独立安装。平时脚本中文件的校验还是存在着一定的价值的。

***

### 参数

* `--apple` 输出旧版MacOS使用的文件类型和创建者代码。代码由八个字母组成，第一个字母描述文件类型，第二个字母描述创建者。

* `-b, --brief` 输出结果中不显示文件名称。

* `-C, --compile` 由`-m`指定的文件编译

* `-c, --checking-printout` 用于检验magic-file输出内容的解析形式，和`-m`一起用于新magic-file的调试。

* `-d` 将内部调试信息输出到stderr。

* `-D, --debug` 输出调试信息。

* `-E` 在文件系统错误（找不到文件等）处理方式上，不要像POSIX要求的那样将错误作为常规输出处理并继续，而是发出错误消息并退出。

* `-e, --exclude` TEST  从为确定文件类型而进行的测试列表中排除testname中命名的测试。

* `-f, --files-from` FILE 从文件中读取要检查的文件名

* `-F, --separator STRING`  使用指定的字符串作为文件名和返回的文件结果之间的分隔符。默认为“：”。

* `-i` 不细分常规文件。

* `-I, --mime`   输出MIMI信息 (--mime-type 和 --mime-encoding)

    * `--mime-type` 输出MIME type
    * `--mime-encoding` 输出MIME encoding


* `-k, --keep-going` 不停止第一次的类型匹配。

* `-l, --list` 列举文件状态列表. （列表较长，自己命令行测试）

* `-L, --dereference` 指向文件链接到目标文件，这是默认选项。

* `-h, --no-dereference` 不指向文件链接到目标文件。

* `-n, --no-buffer` 在检查每个文件后强制刷新stdout。这只有在检查文件列表时才有用。它的用途是希望从管道输出文件类型。

* `-p, --preserve-date` 保留文件的访问时间

* `-r, --raw` 无操作，兼容老版本。

* `-s, --special-files` 将特殊（块/字符设备）文件视为普通的

* `-v, --version` 输出版本信息。

* `-z, --uncompress` 尝试去查看压缩文件。

* `-Z, --uncompress-noreport` 仅显示压缩文件内容, 这个参数有bug。

* `--help` 显示帮助文件。

***

### Example

* `file -b file1`

    不显示文件名称

    ```
    zzw:temp zzw$ file -b file1
    ASCII text
    zzw:temp zzw$ file file1
    file1: ASCII text
    ```

* `file -I file1`

    显示MIME信息

    ```
    zzw:temp zzw$ file -I file1
    file1: text/plain; charset=us-ascii
    zzw:temp zzw$ file -i file1
    file1: regular file
    zzw:temp zzw$ file file1
    file1: ASCII text
    ```

* `file -f filelist`

    多个文件检测时，通过文件列表形式。

    ```
    zzw:temp zzw$ file -f filelist
    file1: ASCII text
    file2: ASCII text
    file3: ASCII text
    zzw:temp zzw$ cat filelist
    file1
    file2
    file3
    ```

* `file -F , file1`

    指定分隔符，默认是冒号。

    ```
    zzw:temp zzw$ file -F , file1
    file1, ASCII text
    ```

* `file -k file1`

    不终止第一次类型匹配的结果。

    ```
    zzw:temp zzw$ file -k file1
    file1: ASCII text
    - data
    ```

* `file -h files_link`

    不指向文件链接的目标文件,和`-L`正好相反。

    ```
    zzw:temp zzw$ file files_link
    files_link: gzip compressed data, last modified: Tue Mar 17 15:06:01 2020, from Unix
    zzw:temp zzw$ file -L files_link
    files_link: gzip compressed data, last modified: Tue Mar 17 15:06:01 2020, from Unix
    zzw:temp zzw$ file -h files_link
    files_link: symbolic link to files.tar.gz
    ```

* `file -z ~/Downloads/Lakka-RPi2.arm-2.3.1.img.gz`

    ```
    zzw:temp zzw$ file ~/Downloads/Lakka-RPi2.arm-2.3.1.img.gz
    /Users/zzw/Downloads/Lakka-RPi2.arm-2.3.1.img.gz: gzip compressed data, was "Lakka-RPi2.arm-2.3.1.img", last modified: Fri Sep 20 11:43:46 2019, from Unix
    zzw:temp zzw$ file -z ~/Downloads/Lakka-RPi2.arm-2.3.1.img.gz
    /Users/zzw/Downloads/Lakka-RPi2.arm-2.3.1.img.gz: DOS/MBR boot sector; partition 1 : ID=0xc, active, start-CHS (0x10,0,1), end-CHS (0x3ff,3,32), startsector 2048, 1048576 sectors; partition 2 : ID=0x83, start-CHS (0x3ff,3,32), end-CHS (0x3ff,3,32), startsector 1050624, 65536 sectors (gzip compressed data, was "Lakka-RPi2.arm-2.3.1.img", last modified: Fri Sep 20 11:43:46 2019, from Unix)
    ```
