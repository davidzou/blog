Mac命令行工具 HomeBrew
=========

古有云：“工欲善其事必先利其器”。那么没有工具怎么来做想做的呢？Windows下到处下个安装文件不就好了，说到这里就不要先说Windows和Mac的安装文件了，常见的Window文件么就是exe和msi之流了。Mac就是pkg和dmg之类了。我个人认为自己是个Terminal的狂热分子，既然为程序猿，那么猿使的东西么还是敲键盘。

闲话短说，直蹦主题，今天讲的是应用管理器，有它不就想要什么软件都可以装了，答案是基本都可以，只有你想不到，没有做不到，当然收费的么有也只有试用版了。Mac下主流的有MacPort和HomeBrew，其实我也就用过这两款。当然我之前是一直用MacPort，但是碍于网速的原因就改用Brew了。不过深入了解后我相信你也会爱不释手的。

##### Step 1： 贴出官网

	https://brew.sh/

##### Step 2： 打开Terminal 敲命令

	/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

	。。。

	安装好了。^\_^ 好了一个强大的软件库摆在你面前了。


“喂， Mac下不是有App Store了嘛，干嘛要这个” By SomeBody

“呃，我知道呀，我是来装逼的” By MySelf

##### **(⊙o⊙)…拿到新东西先看说明书**

```
zzw:github zzw$ brew
Example usage:
  brew search [TEXT|/REGEX/]
  brew info [FORMULA...]
  brew install FORMULA...
  brew update
  brew upgrade [FORMULA...]
  brew uninstall FORMULA...
  brew list [FORMULA...]

Troubleshooting:
  brew config
  brew doctor
  brew install --verbose --debug FORMULA

Contributing:
  brew create [URL [--no-fetch]]
  brew edit [FORMULA...]

Further help:
  brew commands
  brew help [COMMAND]
  man brew
  https://docs.brew.sh
```

看玩说明书当然要实践一下，我想装个软件unrar, 命令行解压缩RAR么都需要的啦。

```
zzw:github zzw$ brew search unrar
==> Formulae
unrar
zzw:github zzw$ brew search koi
==> Formulae
davidzou/koi/koi ✔
```

找到了，是不是我们要找的呢？

```
zzw:github zzw$ brew info unrar
unrar: stable 5.8.1 (bottled)
Extract, view, and test RAR archives
https://www.rarlab.com/
Not installed
From: https://github.com/Homebrew/homebrew-core/blob/master/Formula/unrar.rb
==> Analytics
install: 32,697 (30 days), 62,840 (90 days), 303,117 (365 days)
install_on_request: 23,079 (30 days), 45,875 (90 days), 204,838 (365 days)
build_error: 0 (30 days)
```

就是他了，果断安装. 安装的过程会对库进行一次升级检验（社区活跃的软件就会隔三差五的更新，是不是服务很好，总是用新版本。）记得之前WindowXP刚出来时，老外那边还在用Win98，老外那羡慕的眼神呀--中国人真富有。

```
brew install unrar
```

"我靠，爽就这么简单。" By Somebody

因为Brew是靠git管理的。那么就是说本地的版本索引不会是最新的，那么就需要去更新，当然你执行install的之前会做这么件事。有更新的软件的时候还是需要手动安装的。

```
brew update && brew upgrade
```

“没了，操作结束了？" By Somebody

"对呀，麻雀虽小五脏俱全。几个命令已经够用了，如过想自己发布命令，那么就需要帮助手册中Troubleshooting和Contributing的命令了” By Myself
