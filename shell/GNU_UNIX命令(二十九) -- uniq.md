uniq
=====

***

### 语法

```
uniq [options] file
```

> 去重行命令，但是只能是相邻的行哦。

***

### 参数

* `-c | --count` 按出现次数给行加前缀。

* `-d | --repeated` 只打印重复行，每组一行。

* `-D`  打印所有重复行。

* `--all-repeated[=METHOD]` 类似于-D，但允许用空行分隔组； METHOD={none(default),prepend,separate}

* `-f, --skip-fields=N` 避免比较前N个字段。

* `--group[=METHOD]`    显示所有项，用空行分隔组；METHOD={separate（default），prepend，append，both}

* `-i, --ignore-case`   比较时忽略大小写差异。

* `-s, --skip-chars=N`  避免比较前N个字符。
          
* `-u, --unique`    仅打印唯一行。
              
* `-z, --zero-terminated`   将所有换行符看做为NUL，不作为新行。

* `-w, --check-chars=N` 比较行中不超过N个字符

* `--help`  显示帮助信息

* `--version`   显示版本信息

***

### Example

* `uniq file`

    ```
    root@248c08605ef5:/# uniq file 
    aaaaaaaa
    cccccccc
    bbbbbbbb
    xxxxxxxx
    iiiiiiii
    xxxxxxxx
    root@248c08605ef5:/# cat file
    aaaaaaaa
    cccccccc
    cccccccc
    bbbbbbbb
    xxxxxxxx
    xxxxxxxx
    iiiiiiii
    iiiiiiii
    iiiiiiii
    xxxxxxxx
    ```
  
 * `uniq -d file`
 
    打印重复行。只打印一行。
 
    ```
    root@248c08605ef5:/# uniq -d file
    cccccccc
    xxxxxxxx
    iiiiiiii
    ```
   
* `uniq -D file`

    打印所有的重复行。

    ```
    root@248c08605ef5:/# uniq -D file
    cccccccc
    cccccccc
    xxxxxxxx
    xxxxxxxx
    iiiiiiii
    iiiiiiii
    iiiiiiii
    ```
  
 * `uniq -D --all-repeated=separate  file`
 
    使用分隔符分隔每组重复。
    
    ```
    root@248c08605ef5:/# uniq -D --all-repeated=separate  file
    cccccccc
    cccccccc
    
    xxxxxxxx
    xxxxxxxx
    
    iiiiiiii
    iiiiiiii
    iiiiiiii
    ```

* `uniq -f 1 file`

    ```
    root@248c08605ef5:/# uniq -f 0 file
    aaaaaaaa
    cccccccc
    bbbbbbbb
    xxxxxxxx
    iiiiiiii
    xxxxxxxx
    root@248c08605ef5:/# uniq -f 1 file
    aaaaaaaa
    ```
  
 * `uniq --group=[METHOD] file`
 
    显示所有项，用分隔符分隔。
    ```
    root@248c08605ef5:/# uniq --group=both file
    
    aaaaaaaa
    
    cccccccc
    cccccccc
    
    bbbbbbbb
    
    xxxxxxxx
    xxxxxxxx
    
    iiiiiiii
    iiiiiiii
    iiiiiiii
    
    xxxxxxxx
    
    root@248c08605ef5:/# uniq --group=append file
    aaaaaaaa
    
    cccccccc
    cccccccc
    
    bbbbbbbb
    
    xxxxxxxx
    xxxxxxxx
    
    iiiiiiii
    iiiiiiii
    iiiiiiii
    
    xxxxxxxx
    
    root@248c08605ef5:/# uniq --group=prepend file
    
    aaaaaaaa
    
    cccccccc
    cccccccc
    
    bbbbbbbb
    
    xxxxxxxx
    xxxxxxxx
    
    iiiiiiii
    iiiiiiii
    iiiiiiii
    
    xxxxxxxx 
    ```
   
* `uniq -u file`

    仅输出唯一行。

    ```
    root@248c08605ef5:/# uniq -u file
    aaaaaaaa
    bbbbbbbb
    xxxxxxxx
    ```
  
 * `sort file | uniq`
 
    结合`sort`命令看下差别吧。这个应该也是最常用的结合方式了吧。
    
    ```
    root@248c08605ef5:/# sort file | uniq
    aaaaaaaa
    bbbbbbbb
    cccccccc
    iiiiiiii
    xxxxxxxx
    root@248c08605ef5:/# uniq file | sort
    aaaaaaaa
    bbbbbbbb
    cccccccc
    iiiiiiii
    xxxxxxxx
    xxxxxxxx
    ```