wc
=====

***

### 语法

```
wc [options] files
```

不笑，很正经，只是个计数命令，计算行数好像用的最多。

***

### 参数

* `-c | --bytes`    显示字节数。

* `-m | --chars`     显示字符数。

* `-l | --lines`     显示新行数。

* `--files0-from=F`     从文件F中以NUL结尾的名称指定的文件读取输入；如果F是-则从标准输入读取名称
      
* `-L | --max-line-length`   显示最大显示宽度

* `-w | --words`     显示单词数。

* `--help`  显示帮助信息。

* `--version`   显示版本信息。

***

### Example

* `wc file`

     标准输出，3个数字分别表示file文件的行数、单词数，以及该文件的字节数。

    ```
    root@ecd79fccf553:/# wc file 
      6  19 110 file 
    root@ecd79fccf553:/# cat file 
    line1 with a space.
    line2 .
    line3.
    with a tab	line4.
    with four space    line5.
    with eight space        line6.
    ```
  
 * `wc -l file`
 
    仅行数。
 
    ```
    root@ecd79fccf553:/# wc -l file 
    6 file 
    ```
   
  * `wc -w file`
  
    仅单词数。
  
    ``` 
    root@ecd79fccf553:/# wc -w file 
    19 file
    ```
    
   * `wc -c file `
   
    仅字符数。
    
    ``` 
    root@ecd79fccf553:/# wc -c file 
    110 file
    root@ecd79fccf553:/# wc -m file 
    110 file 
    ```
  
  * `wc -L file`
  
    最宽的行。
  
    ```
    root@ecd79fccf553:/# wc -L file 
    30 file 
    ```