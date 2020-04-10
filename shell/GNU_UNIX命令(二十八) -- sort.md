sort
=====

***

### 语法

```
sort [options] file
```

> 很简单却是很难玩精的一条命令，设计之初可能就考虑了很多。

***

### 参数

* `-b | --ignore-leading-blanks`

    在每行中查找排序键时忽略前导空格。默认情况下，空白是空格或制表符，但 __LC_CTYPE__ 区域设置可以更改此值。注意：区域设置的排序规则可能会忽略空白，但如果没有此选项，空白对于在带有-k选项的键中指定的字符位置将非常重要。

* `-c | --check | --check=diagnose-firt`  

    检查给定文件是否已排序：如果未全部排序，则打印包含第一个无序行的诊断，并以1状态退出。否则，退出成功。最多只能给出一个输入文件。

* `-C | --check=quiet | --check=slient`

    如果给定文件已排序，则成功退出，否则退出并返回状态1。最多只能给出一个输入文件。这就像`-c`，只是它不打印诊断。

* `-d | --dictinary-order`

    按电话簿顺序排序：排序时忽略除字母、数字和空格以外的所有字符。默认情况下，字母和数字是 __ASCII__ 的字母和数字，空白是空格或制表符，但是 __LC_CTYPE__ 语言环境可以更改这一点。

* `-f | --ignore-case`  

    不区分大小写。在进行比较时，将小写字符折叠为等效的大写字符，以便`b`和`B`排序为相等。__LC_CTYPE__ 区域设置确定字符类型。当与`--unique`一起使用时，那些小写的等价行将被丢弃。（目前还没有办法扔掉大写的等价物。（任何——倒数只会影响最终结果，在扔掉之后。）

* `-g | --general-numeric-sort | --sort=general-numeric`

    按数字排序，将每行的前缀转换为长双精度浮点数。不报告溢出、下溢或转换错误。使用以下排序序列：

    - 不以数字开头的行（都被认为是相等的）。
    - 以一致但与机器相关的顺序排列的NaNs（在IEEE浮点运算中为“非数字”值）。
    - 负无穷大。
    - 以升序数字顺序排列的有限数（其中-0和+0相等）
    - 正无穷大。

    只有在没有其他选项的情况下才使用此选项；它比--数字排序（`-n`）慢得多，并且在转换为浮点时可能丢失信息。

* `-h | --human-numeric-sort | --sort=human-numeric`

    按数字顺序排序，首先按数字符号（负号、零号或正号）排序；然后按 __SI__ (Systeme International国际单位制)后缀（空后缀或`k`或`K`后缀或`MGTPEZY`后缀之一）排序；最后按数值排序。例如，__1023M__ 在 __1G__ 之前排序，因为`M`（mega）在`G`（giga）之前作为 __SI__ 后缀。

* `-i | --ignore-nonprinting`

    忽略非打印字符。__LC_CTYPE__ 区域设置确定字符类型。如果还提供了更强大的`--dictionary order（-d）`选项，则此选项无效。

* `-m | --merge`   

    通过将给定文件作为一个组进行排序来合并它们。每个输入文件必须始终单独分类。它总是以排序而不是合并的方式工作；提供合并是因为它在工作的情况下速度更快。

* `-M | --month-sort | --sort=month`

    一个初始字符串，由任意数量的空格组成，后跟一个月名称缩写，转换为大写，并按 `JAN < FEB < ... < DEC`。无效名称低于有效名称。__LC_TIME__ locale类别确定月份拼写。默认情况下，空白是空格或制表符，但 __LC_CTYPE__ 区域设置可以更改此值。

* `-n | --numeric-sort | --sort=numeric`

    按数字排序。数字从每一行开始，由可选空格、可选`-`符号和可能由数千个分隔符分隔的零个或多个数字组成，可选后跟小数点字符和零个或多个数字。空数字被视为`0`。__LC_NUMERIC__ 区域设置指定小数点字符和千位分隔符。默认情况下，空白是空格或制表符，但 __LC_CTYPE__ 区域设置可以更改此值。比较准确；没有舍入误差。无法识别前导`+`或指数表示法。要以数字方式比较这些字符串，请使用`--general-numeric-sort（-g）`选项。

* `-r | --reverse`

    反转比较结果，使键值越大的行在输出中出现得越早，而不是越晚。

* `-R | --random-sort | --sort=random`

    通过对输入键进行哈希排序，然后对哈希值进行排序。随机选择散列函数，确保它没有冲突，以便不同的键具有不同的散列值。这类似于输入的随机排列，只是具有相同值的键排序在一起。如果指定了多个随机排序字段，则对所有字段使用相同的随机哈希函数。要对不同字段使用不同的随机哈希函数，可以多次调用sort。哈希函数的选择受`--random-source`选项的影响。

* `--sort=WORD`

    排序命令，上面命令中`--sort=`的都是。多项选择，看你个人习惯。

* `-V  | --version-sort`

    按版本名称和编号排序。它的行为类似于标准排序，只是每一个十进制数字序列在数字上被视为一个索引/版本号。


***

### Example

*  `sort`

    标准排序按照字幕顺序。

    ```
    root@5ca8dbe55d8d:/test# cat file1
    line1
    line2
    line3

    line4
    line5
    root@5ca8dbe55d8d:/test# cat file1 | sort

    line1
    line2
    line3
    line4
    line5
    ```

* `sort -k 6 -n`

    显示系统进程，以常驻部分的大小(RSS字段)顺序列出。

    ```
    root@5ca8dbe55d8d:/test# ps aux | sort -k 6 -n
    USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
    root       444  0.0  0.0  18616   500 pts/0    R+   08:04   0:00 /bin/bash
    root       443  0.0  0.1  34404  2824 pts/0    R+   08:04   0:00 ps aux
    root         1  0.0  0.1  18616  3456 pts/0    Ss   04:59   0:00 /bin/bash
    ```

* `sort -n -r file1`

    反转排序数字。

    ```
    root@e0d7f2da343a:/# sort -n -r file1
    6
    5
    4
    3
    2.
    1
    root@e0d7f2da343a:/# cat file1
    1
    2.
    3
    4
    5
    6
    root@e0d7f2da343a:/#
    ```

* `sort -t : -n -k 5b,5 -k 3,3 /etc/passwd`

    对第五个字段上的密码文件进行排序，并忽略任何前导空格。对字段3中数字用户ID的字段5中的值相等的行进行排序。字段用“：”分隔。

    ```
    root@e0d7f2da343a:/# sort -t : -n -k 5b,5 -k 3,3 /etc/passwd
    _apt:x:100:65534::/nonexistent:/usr/sbin/nologin
    gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
    list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
    backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
    bin:x:2:2:bin:/bin:/usr/sbin/nologin
    daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
    games:x:5:60:games:/usr/games:/usr/sbin/nologin
    irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
    lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
    mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
    man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
    news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
    nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
    proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
    root:x:0:0:root:/root:/bin/bash
    sync:x:4:65534:sync:/bin:/bin/sync
    sys:x:3:3:sys:/dev:/usr/sbin/nologin
    uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
    www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
    ```
