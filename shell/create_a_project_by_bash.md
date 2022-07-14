将Bash命令工程化
======

我们平时喜欢使用脚本来处理一些事情，但是你会发现每次最多都是修修改改一个文件，却没有将这个命令实际作为一个项目来处理。无论使用什么语言，工程化，系统化
是解决最后麻烦产生的最简单方法，随着功能的无限扩张，语法修正也罢。

## Hello World

``` 
$ vi helloworld

#!/bin/bash
echo "Hello World!"

$ bash helloworld
    Hello World!
```

至此一个最简单的脚本文件出现了。

## Help

一条命令我们都知道离不开参数，那么可以实现不同的逻辑处理。其中最常用的就是--help这个参数。我就来看看是怎么实现的。

```
#!/bin/bash
if [[ $1 == '--help' ]] ; then
    echo "This is help, why say Hello World?"
else 
    echo "Hello World!"
fi 
```

## 工程化

一个项目需要工程化，就算在小，它也有存在的意义，不能用过结束，否则就变得重复造车轮，有害而无一益。


``` 
├── ges/                                        项目主目录
│ ├── CHANGELOG.md                              更新日志
│ ├── README.md                                 
│ ├── build.gradle                              项目构建脚本
│ ├── build.sh                                  项目执行构建脚本
│ ├── libs/                                     其他引用的库或者源码
│ │ └── ges.jar
│ ├── res/                                      资源文件目录
│ │ └── ges.gradle
│ └── src/                                      源码目录
│     ├── common/                               通用方法目录，一般如命令检测，环境检测，计算等
│     ├── function/                             项目方法目录，自定义方法
│     ├── ges.sh                                主命令，即我们常说的main函数就是它。
│     ├── install.sh                            安装脚本，命令打包完成后，安装脚本用于安装到计算机，使其可被方便执行。
│     ├── options/                              选项目录，也就是我们帮助文档和帮助参数的设置
│     │ ├── ges_option_help.txt                 帮助文档，你想要展示给别人看的帮助信息
│     │ └── ges_options.sh                      命令参数处理逻辑
│     └── uninstall.sh                          卸载命令，安装后，用于卸载用，删除缓存，配置信息之类的。

```

以上目录结构包含了一个脚本命令的基本开发流程所涉及的一些内容的定义，特殊的需要按照各自的需求进行扩容和开发。

待续...