谈谈Linux系统目录
====

***

### 历史

Linux的开放导致了很多的“发行版”，导致的一个最严重的问题就是文件系统布局的不一致。为了有一个公认的标准，Linux社群于1993年发起了一个计划，打算定制一套适合一般发行版的文件系统布局标准。该计划于1994年形成，称为Linux Filesystem Standard（简称FSSTND）。这份草案标准获得了广泛的响应，甚至被认为应该被提升到更高的层次，成为所有Unix-like操作系统的标准。后来消减了其中原本针对Linux而设计的部分，成为后来的[Filesystem Hierarchy Standard（简称FHS）](http://www.pathname.com/fhs/)，2004年发布了2.3版本。

***

### 数据类型

FHS将数据的属性分为两类[^filesystem]

    * 可共享的（shareable）和不可共享的（unshareable）

        * `shareable` 可供网络上的多个主机系统同时访问的数据。通常是无关特定主机的一般性信息，诸如用户的数据文件、可执行的程序文件以及通用的配置文件。

        * `unshareable` 仅供特定主机使用的数据。通常是仅对特定主机才有意义的信息，像`passwd`文件、网络配置文件、系统日志文件等。

    * 可变的（variable）和静态不变的（static）

        * `variable` 会因系统运作或认为操作而经常变化的数据。例如：用户的系统日志文件。

        * `static` 除非人为的可以操作，否则平常不会有变化的数据。例如：二进制命令程序（像`ls`），这类文件只有在系统管理者进行版本升级时才有可能被修改。

***

### 根目录`/`

根目录位于整个目录树的最顶端。依据FHS的定义[^root_filesystem]，根目录应该满足以下条件：

* 必须含有足以启动操作系统的工具程序和文件，包括挂载其他文件系统的能力。

* 必须具备系统管理者修理和恢复遭损系统所需要的工具程序。

* 规模应该精简。

* 应用软件不应该在根目录上创建文件或者目录。

### 根目录应该有的基本目录[^require]

* `/bin`

    系统的基本命令，像是`cp`,`ls`,`ln`,`mkdir`等。此目录应该含有解决问题所需的工具。

* `/boot`

    存放内核镜像文件，boot loader所需的文件。

* `/dev`

    设备文件，这是访问磁盘与其他设备所需要的。

* `/etc`

    个别系统的配置信息，特别是启动时期会用到的信息必须齐全，例如：`passwd`,`hosts`等。

* `/lib`

    存放共享链接库以及内核模块。这些都是系统进行初始化时所必需的的。

* `/media`

    它是用来挂载插入式储媒。具有和`/mnt`相同的作用。

* `/mnt`

    这是个空目录，它只包含几个挂载点，用以挂载临时的文件系统。

* `/opt`

    主要用于安装非操作系统所安装的额外软件。第三方软件商常会选用这个未知来安装他们的软件。

* `/sbin`

    用于系统管理的基本工具。例如：`fdisk`,`fsck`以及`mkfs`等。

* `/srv`

    系统服务的专属数据。这是FHS2.3才提出的，是让用户可以找到特定系统服务的数据文件以及相关的脚本。

* `/tmp`

    存放临时文件。

* `/usr`

    存放应用软件系统。

* `/var`

    存放随时间改变的数据，像日志文件。


**`/usr`和`/var`的目录结构其实还值得深入探讨。**

***

### 参考

[^filesystem]: [FHS -- Chapter 2. The Filesystem](http://www.pathname.com/fhs/pub/fhs-2.3.html#THEFILESYSTEM)

[^root_filesystem]: [FHS -- Chapter 3. The Root FileSystem](http://www.pathname.com/fhs/pub/fhs-2.3.html#THEROOTFILESYSTEM)

[^require]: [FHS -- The Root FileSystem Requirements](http://www.pathname.com/fhs/pub/fhs-2.3.html#REQUIREMENTS)
