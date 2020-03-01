# ls命令

列出目录内容

`ls [选项] [参数]`

### 参数

* -a, –all 列出目录下所有的内容，包含'.'(当前目录)和'..'(上一级目录)的隐藏文件
* -A 	同-a，但不列出“.”(表示当前目录)和“..”(表示当前目录的父目录)。
* -l 	除了文件名之外，还将文件的权限、所有者、文件大小等信息详细列出来。
* -f 	对输出的文件不进行排序，-aU 选项生效，-lst 选项失效
* -g 	类似 -l,但不列出所有者
* -h 	–human-readable 以容易理解的格式列出文件大小 (例如 1K 234M 2G)
* –si 	类似 -h,但文件大小取 1000 的次方而不是 1024
* -k 	即 –block-size=1K,以 k 字节的形式表示文件的大小。
* -L	–dereference 当显示符号链接的文件信息时，显示符号链接所指示的对象而并非符号链接本身的信息
* -m 	所有项目以逗号分隔，并填满整行行宽
* -o 	类似 -l,显示文件的除组信息外的详细信息。   
* -r	–reverse 依相反次序排列
* -R	–recursive 同时列出所有子目录层
* -s 	–size 以块大小为单位列出所有文件的大小
* -S 	根据文件大小排序
* -t 	以文件修改时间排序
* -u 	配合 -lt:显示访问时间而且依访问时间排序 
	  	配合 -l:显示访问时间但根据名称排序
		否则：根据访问时间排序
* -U 	不进行排序;依文件系统原有的次序列出项目
* -1 	每行只列出一个文件
* –help 显示此帮助信息并离开
* –version 显示版本信息并离开

### Example

* ```ls```

	平时打的最多的就是他了，简单的显示当前目录下的文件内容
	
	```
	zzw:dailyshell davidzou$ ls
	adir         afile        asubfilelink bfile
	```

* ```ls -al```

	列出所有的内容
	
	```
	zzw:dailyshell davidzou$ ls -al
	drwxr-xr-x  6 davidzou  staff  192 12  7 01:08 .
	drwxr-xr-x  5 davidzou  staff  160 12  7 01:06 ..
	drwxr-xr-x  4 davidzou  staff  128 12  7 01:07 adir
	-rw-r--r--  1 davidzou  staff   14 12  7 01:07 afile
	lrwxr-xr-x  1 davidzou  staff   13 12  7 01:08 asubfilelink -> adir/asubfile
	-rw-r--r--  1 davidzou  staff   14 12  7 01:07 bfile
	```

*  ```ls -Al```
	列出除'.'和'..'以外的所有内容

	```
	zzw:dailyshell davidzou$ ls -Al
	drwxr-xr-x  4 davidzou  staff  128 12  7 01:07 adir
	-rw-r--r--  1 davidzou  staff   14 12  7 01:07 afile
	lrwxr-xr-x  1 davidzou  staff   13 12  7 01:08 asubfilelink -> adir/asubfile
	-rw-r--r--  1 davidzou  staff   14 12  7 01:07 bfile
	```

* ```ls -tl```

	列出所有内容，并以时间排序（倒序）
	
	```
	zzw:dailyshell davidzou$ ls -tl
	lrwxr-xr-x  1 davidzou  staff   13 12  7 01:08 asubfilelink -> adir/asubfile
	drwxr-xr-x  4 davidzou  staff  128 12  7 01:07 adir
	-rw-r--r--  1 davidzou  staff   14 12  7 01:07 bfile
	-rw-r--r--  1 davidzou  staff   14 12  7 01:07 afile
	```

* ```ls -trl```

	列出所有内容，并以时间排序（正序）
	
	```
	zzw:dailyshell davidzou$ ls -trl
	-rw-r--r--  1 davidzou  staff   14 12  7 01:07 afile
	-rw-r--r--  1 davidzou  staff   14 12  7 01:07 bfile
	drwxr-xr-x  4 davidzou  staff  128 12  7 01:07 adir
	lrwxr-xr-x  1 davidzou  staff   13 12  7 01:08 asubfilelink -> adir/asubfile
	```
	
* ```ls -lR```

	列出所有内容，包含子目录下的内容
	
	```
	zzw:dailyshell davidzou$ ls -lR
	drwxr-xr-x  4 davidzou  staff  128 12  7 01:07 adir
	-rw-r--r--  1 davidzou  staff   14 12  7 01:07 afile
	lrwxr-xr-x  1 davidzou  staff   13 12  7 01:08 asubfilelink -> adir/asubfile
	-rw-r--r--  1 davidzou  staff   14 12  7 01:07 bfile
	
	./adir:
	-rw-r--r--  1 davidzou  staff  17 12  7 01:07 asubfile
	-rw-r--r--  1 davidzou  staff  17 12  7 01:07 bsubfile
	```
	
* ```ls -gl```

	列出所有内容，但没有所有者一栏
	
	```
	zzw:dailyshell davidzou$ ls -gl
	drwxr-xr-x  4 staff  128 12  7 01:07 adir
	-rw-r--r--  1 staff   14 12  7 01:07 afile
	lrwxr-xr-x  1 staff   13 12  7 01:08 asubfilelink -> adir/asubfile
	-rw-r--r--  1 staff   14 12  7 01:07 bfile
	```
	
* ```ls -lh```
	
	列出所有内容，文件大小以单位表示
	
	```
	zzw:dailyshell davidzou$ ls -hl
	drwxr-xr-x  4 davidzou  staff   128B 12  7 01:07 adir
	-rw-r--r--  1 davidzou  staff    14B 12  7 01:07 afile
	lrwxr-xr-x  1 davidzou  staff    13B 12  7 01:08 asubfilelink -> adir/asubfile
	-rw-r--r--  1 davidzou  staff    14B 12  7 01:07 bfile
	```
	
* ```ls -m```

	列出所有内容，以逗号分隔
	
	```
	zzw:dailyshell davidzou$ ls -m
	adir, afile, asubfilelink, bfile
	```
	
* ```ls -Sl```

	列出所有内容，以文件大小排列
	
	```
	zzw:dailyshell davidzou$ ls -Sl
	drwxr-xr-x  4 davidzou  staff  128 12  7 01:07 adir
	-rw-r--r--  1 davidzou  staff   14 12  7 01:07 afile
	-rw-r--r--  1 davidzou  staff   14 12  7 01:07 bfile
	lrwxr-xr-x  1 davidzou  staff   13 12  7 01:08 asubfilelink -> adir/asubfile
	```
	
* ```ls -1```

	列出所有，但每行只显示1个

	```
	zzw:dailyshell davidzou$ ls -1
	adir
	afile
	asubfilelink
	bfile
	```