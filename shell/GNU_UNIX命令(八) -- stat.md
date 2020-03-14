stat
====

***

### 语法

```
stat [options] file ...
```

> stat是查看文件状态，而ls只是查看目录文件列表。请注意区分。

***

### 参数

* `-F` 在每个目录之后显示斜杠（ / ），在每个可执行文件之后显示星号（ \* ），在每个链接后显示（ @ ），在每个whiteout文件之后有一个百分号（ % ），在每个socket之后有一个等号（ = ），在每个FIFO之后有一个竖线（ | ）。-F默认使用-l。
* `-f` 自定义格式化显示信息。
* `-L` 如果文件是链接，stat显示的是引用的文件，而不是文件本身。
* `-l` 使用ls -lT格式显示。
* `-n` 不要强迫换行符出现在每个输出的末尾。
* `-r` 显示原始信息
* `-s` 在“shell输出”中显示信息，适用于初始化变量。
* `-t` 使用指定格式显示时间戳。
* `-x` 以更详细的方式显示信息，如某些Linux发行版中所知。

***

### Example

* `stat file1`

    ```
    zzw:temp zzw$ stat file1
    16777221 69282173 -rw-r--r-- 1 zzw staff 0 6 "Mar 12 23:56:26 2020" "Mar 12 23:56:25 2020" "Mar 12 23:56:25 2020" "Mar  1 18:00:00 2020" 4096 8 0 file1
    ```

* `stat -r file1`

    显示原始信息

    ```
    zzw:temp zzw$ stat -r file1
    16777221 69282173 0100644 1 501 20 0 6 1584028586 1584028585 1584028585 1583056800 4096 8 0 file1
    ```

* `stat -s file1`

    将输出作为shell的输出变量

    ```
    zzw:temp zzw$ stat -s file1
    st_dev=16777221 st_ino=69282173 st_mode=0100644 st_nlink=1 st_uid=501 st_gid=20 st_rdev=0 st_size=6 st_atime=1584028586 st_mtime=1584028585 st_ctime=1584028585 st_birthtime=1583056800 st_blksize=4096 st_blocks=8 st_flags=0
    zzw:temp zzw$ eval $(stat -s file1)
    zzw:temp zzw$ echo $st_ino
    69282173
    ```

* `stat -x file1`

    ```
    zzw:temp zzw$ stat -x file1
      File: "file1"
      Size: 6            FileType: Regular File
      Mode: (0644/-rw-r--r--)         Uid: (  501/     zzw)  Gid: (   20/   staff)
    Device: 1,5   Inode: 69282173    Links: 1
    Access: Thu Mar 12 23:56:26 2020
    Modify: Thu Mar 12 23:56:25 2020
    Change: Thu Mar 12 23:56:25 2020
    ```

* `stat -F`

    ```
    zzw:temp zzw$ stat -F
    crw--w---- 1 zzw tty 16,2 Mar 12 23:53:31 2020 (stdin)
    zzw:temp zzw$ stat -F .
    drwxr-xr-x 5 zzw staff 160 Mar 12 23:45:43 2020 ./
    zzw:temp zzw$ stat -F file1
    -rw-r--r-- 1 zzw staff 0 Mar 12 23:35:02 2020 file1
    zzw:temp zzw$ stat -F file1_link
    lrwxr-xr-x 1 zzw staff 5 Mar 12 23:38:02 2020 file1_link@ -> file1
    zzw:temp zzw$ stat -F command
    -rwxr-xr-x 1 zzw staff 7 Mar 12 23:56:06 2020 command*
    ```

* `stat -f "%N: %HT%SY" ./*`

    自定义格式输出

    ```
    zzw:temp zzw$ stat -f "%N: %HT%SY" ./*
    ./command: Regular File
    ./file1: Regular File
    ./file1_link: Symbolic Link -> file1
    ./src: Directory
    ```
