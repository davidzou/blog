Mac命令行工具 HomeBrew
===========

知道HomeBrew的功能后,那么就可以去下载安装各种各样的命令，我还是那句话，程序猿就是用命令行，鼠标那东西只会减低你的效率，拉低你的档次。
不过有时不是所有的命令都能满足特殊需求的，那么就需要自己写一个命令，既然好东西么就要分享出来，别藏着捏着的，这篇讲的就是通过brew发布
自有的命令。

## HomeBrew类型

当我们使用搜索功能时，通常会有如下3种类型的可安装方式

```
zzw:blog zzw$ brew search info
==> Formulae
clinfo                        inform6                       jpeginfo                      media-info                    pinfo                         texinfo
davidzou/apkinfo/apkinfo ✔    ipinfo                        libosinfo                     mp3info                       shared-mime-info ✔

==> Casks
bdinfo                              cpuinfo                             inform                              mediainfo                           pdfinfo
```

* Formulae 即homebrew的核心库

`==> Formulae`下单个名称的， 安装方式也相对简单，直接brew install [NAME]即可

```
zzw:blog zzw$ brew install jpeginfo
Updating Homebrew...
==> Downloading https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles/bottles/jpeginfo-1.6.1_1.high_sierra.bottle.tar.gz
######################################################################## 100.0%
==> Pouring jpeginfo-1.6.1_1.high_sierra.bottle.tar.gz
🍺  /usr/local/Cellar/jpeginfo/1.6.1_1: 7 files, 49KB
```

* Casks 即homebrew的补充库

`==> Casks`下的应用，安装方式也较方便，直接brew cask install [NAME]即可

* Tap  即个人自定义库

`davidzou/apkinfo/apkinfo` 类似这类名称的，安装的话就稍多一个步骤，因为是基于github的托管方式，那么就需要pull一下啦。

```
brew tap davidzou/apkinfo
brew install apkinfo
```

> 这里也就是我今天要讲的如何提交一个自定义命令由brew来托管安装。

### 如何通过HomeBrew安装发布自定义的脚本命令

1. 创建项目 project的命名规则需`homebrew-`前缀命名

	```
	基本的项目结构如下：
	project/|
	       +- Formula/ (Brew安装默认) <font color=red>*必须</font>
	       |         +- xxx.rb (Brew描述文件)
	       +- xxx.sh (命令文件)
	       +- xxx.1  (man文件)
	       +- tarball/ (自定义发布用安装压缩包)
	```

2. 创建命令脚本

	helloworld.sh

	```
		#!/bin/bash
		echo "Hello World!"
	```

3. 创建man

	hellowrold.1

	```
		.\" Manpage for nuseradd.
		.\" Contact vivek@nixcraft.net.in to correct errors or typos.
		.TH man 8 "06 May 2010" "1.0" "nuseradd man page"
		.SH NAME
		nuseradd \- create a new LDAP user
		.SH SYNOPSIS
		nuseradd [USERNAME]
		.SH DESCRIPTION
		nuseradd is high level shell program for adding users to LDAP server.  On Debian, administrators should usually use nuseradd.debian(8) instead.
		.SH OPTIONS
		The nuseradd does not take any options. However, you can supply username.
		.SH SEE ALSO
		useradd(8), passwd(5), nuseradd.debian(8)
		.SH BUGS
		No known bugs.
		.SH AUTHOR
		Vivek Gite (vivek@nixcraft.net.in)
	```

4. 创建homewbrew描述文件

	```
		class Apkinfo < Formula
		  homepage ''
		  url 'your source addreess'
		  desc 'xxx'
		  sha256 'no-check'
		  version '0.0.1'

		  def install
		    bin.install "xxx.sh" => "xxx"
		    man1.install "xxx.1"
		  end
		end
	```

5. 打包

	```
		tar cvf xxx.tar xxx.sh xxx.1
	```

6. 生成sha256校验

	```
		shasum -a 256 xxx.tar

		将此命令获得的值替换xxx.rb 中sha256后的内容
	```
7. 提交github
