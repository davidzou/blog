diff
====

***

### 语法

```
diff [options] ... files
```

> 一行一行的比较文件，是不是感觉Torvalds也复用了些什么？

***

### 参数

* `-i --ignore-case` 忽略文件内容中的大小写差异。

* `--ignore-file-name-case` 比较文件名时忽略大小写。

* `--no-ignore-file-name-case` 比较文件名时考虑大小写。

* `-b  --ignore-space-change` 忽略空白字符。

* `-w  --ignore-all-space` 忽略所有空白。

* `-B  --ignore-blank-lines` 忽略行全部空白行。

* `-I RE  --ignore-matching-lines=RE` 忽略所有行都匹配RE(指定字符)的。

* `-a  --text` 将所有文件视为文本。

* `-c  -C NUM  --context[=NUM]` -C 复制的上下文的输出NUM（默认3）行, -c 输出全部。

* `-u  -U NUM  --unified[=NUM]` -U 输出统一上下文的NUM行（默认为3行），-u 输出全部。

* `--label LABEL` 使用LABEL而不是文件名

* `-p  --show-c-function` 若比较的文件为C语言的程序码文件时，显示差异所在的函数名称。

* `-F RE  --show-function-line=RE` 显示最近匹配RE(指定字符)的行。

* `-q  --brief` 仅输出文件是否不同。

* `-e  --ed` 此参数的输出格式可用于ed的script文件。

* ` --normal` 仅输出普通的一种比较

* `-n  --rcs` 将比较结果以RCS的格式来显示。

* `-y  --side-by-side` 以并列的方式显示文件的异同之处。

* `-W NUM  --width=NUM` 在使用-y参数时，指定栏宽(默认130)。

* `--left-column` 在使用-y参数时，仅指定左边栏宽。

* `--suppress-common-lines` 在使用-y参数是，不输出无差别内容行。

* `-D NAME  --ifdef=NAME` 输出合并文件以显示“#ifdef NAME”差异。

* `-l  --paginate` 将结果交由pr程序来分页。

* `-t  --expand-tabs` 展开tab输出为空格。

* `-T  --initial-tab` 在每行前面加上tab字符以便对齐。

* `-r  --recursive` 递归地比较找到的任何子目录。

* `-N  --new-file` 将缺少的文件视为空。

* ` --unidirectional-new-file` 将缺少的第一个文件视为空。

* `-s  --report-identical-files` 当两个文件相同时报告。

* `-x PAT  --exclude=PAT` 不比较选项中所指定的文件或目录。

* `-X FILE  --exclude-from=FILE` 排除与文件中的任何模式匹配的文件。

* `-S FILE  --starting-file=FILE` 在比较目录时，从指定的文件开始比较。

* `--from-file=FILE1` 将FILE1与所有操作进行比较。FILE1可以是目录。

* `--to-file=FILE2` 将所有操作与FILE2进行比较。FILE2可以是目录。

* `-d  --minimal` 试图寻找较小的改变。

* `--speed-large-files` 假设有大文件和许多零散的小更改。

* `-v  --version` 输出版本信息。

* `--help` 输出帮助信息

***

### Example

* `diff file1 file2`

    没有任何参数比较两个文件。

    ```
    zzw:temp zzw$ diff file1 file2
    1,2c1,2
    < file1
    < a
    ---
    > file2
    > A
    4c4
    < line4
    ---
    > Line4
    zzw:temp zzw$ cat file1
    file1
    a
    line3
    line4
    zzw:temp zzw$ cat file2
    file2
    A
    line3
    Line4
    ```

* `diff -i file1 file2`

    忽略大小写比较

    ```
    zzw:temp zzw$ diff -i file1 file2
    1c1
    < file1
    ---
    > file2
    ```

* `diff -I ine file1 file2`

    忽略行中包含了ine的，这里`line4`和`Line4`的比较就和`-i`效果一样了

    ```
    zzw:temp zzw$ diff -I ine file1 file2
    1,2c1,2
    < file1
    < a
    ---
    > file2
    > A
    ```

* `diff -u file1 file2`

    以合并的方式标识不同，是不是很熟悉。

    ```
    zzw:temp zzw$ diff -u file1 file2
    --- file1	2020-03-21 23:56:50.000000000 +0800
    +++ file2	2020-03-21 23:56:59.000000000 +0800
    @@ -1,4 +1,4 @@
    -file1
    -a
    +file2
    +A
     line3
    -line4
    +Line4
    ```

* `diff -c file1 file2`

    标识每行不同，不同的行前面有`!`。

    ```
    zzw:temp zzw$ diff -c file1 file2
    *** file1	2020-03-21 23:56:50.000000000 +0800
    --- file2	2020-03-21 23:56:59.000000000 +0800
    ***************
    *** 1,4 ****
    ! file1
    ! a
      line3
    ! line4
    --- 1,4 ----
    ! file2
    ! A
      line3
    ! Line4
    ```

* `diff -u --label LABEL file1 file2`

    使用`--label`标签后file1的文件名就变成了自定义的LABEL

    ```
    zzw:temp zzw$ diff -u --label LABEL file1 file2
    --- LABEL
    +++ file2	2020-03-22 00:57:31.000000000 +0800
    @@ -1,4 +1,4 @@
    -file1
    -a
    +file2
    +A
     line3
    -line4
    +Line4
    ```

* `diff -w file1 file2`

    忽略所有的空白符，第五行添加了一个tab符在`file1`中.

    ```
    zzw:temp zzw$ diff -w file1 file2
    1,2c1,2
    < file1
    < a
    ---
    > file2
    > A
    4c4
    < line4
    ---
    > Line4
    zzw:temp zzw$ cat file1
    file1
    a
    line3
    line4
    	line5
    zzw:temp zzw$ cat file2
    file2
    A
    line3
    Line4
    line5
    ```

* `diff -q file1 file2`

    仅仅输出两文件是否不同。

    ```
    zzw:temp zzw$ diff -q file1 file2
    Files file1 and file2 differ
    ```

* `diff -n file1 file2`

    ```
    zzw:temp zzw$ diff -n file1 file2
    d1 2
    a2 2
    file2
    A
    d4 2
    a5 2
    Line4
    line5
    ```

* `diff -y file1 file2`

    以并列的方式显示，`|`代表有差别

    ```
    zzw:temp zzw$ diff -y file1 file2
    file1							      |	file2
    a							          |	A
    line3								    line3
    line4							      |	Line4
    	line5						      |	line5
    ```

* `diff -y --suppress-common-lines file1 file2`

    `--suppress-common-lines`不会输出line3，因为内容一样。

    ```
    zzw:temp zzw$ diff -y --suppress-common-lines file1 file2
    file1							      |	file2
    a							          |	A
    line4							      |	Line4
    	line5						      |	line5
    ```

* `diff -s file3 file4`

    当两个文件相同是显示信息，否则显示不同。不加`-s`情形下，两文件想不无任何输出信息。

    ```
    zzw:temp zzw$ diff -s file3 file4
    Files file3 and file4 are identical
    ```

* `diff -v`

    输出版本信息。

    ```
    zzw:temp zzw$ diff -v
    diff (GNU diffutils) 2.8.1
    Copyright (C) 2002 Free Software Foundation, Inc.

    This program comes with NO WARRANTY, to the extent permitted by law.
    You may redistribute copies of this program
    under the terms of the GNU General Public License.
    For more information about these matters, see the file named COPYING.

    Written by Paul Eggert, Mike Haertel, David Hayes,
    Richard Stallman, and Len Tower.
    ```
