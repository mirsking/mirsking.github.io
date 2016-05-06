---
layout: post
title:  "ubuntu12.04安装vim7.4 with lua的心酸历程"
date: 2014-05-09 19:42 +0800
categories:  工具配置
---

因团队需要，忍痛抛弃了ubuntu14.04 LTS，改回ubuntu12.04，允许我吐槽一分钟：

第一就是搜狗和麒麟合力打造的新版linux版输入法，简单`sudo dpkg -i`，结果到ubuntu12.04这却要ppa升级fcitx。升级玩的fcitx却各种不稳定，不定时抽风，然后整个输入法就甭想了。

第二是今天的重点，ubuntu14.04安装vim 7.4直接一个`sudo apt-get install vim`就ok了，结果到12.04这，偏偏要给你装vim 7.3。vim
7.3也就算了，偏偏低于vim 7.3.885。结果就是悲剧的无法使用大名鼎鼎的**[neocomplete.vim](https://github.com/Shougo/neocomplete.vim)**插件。本来以为找到了终极的YouCompleteMe插件，结果发现YouCompleteMe是依赖neocomplete.vim的。

本来吧，在ubuntu12.04上装vim 7.4也不是什么难事，ppa升级，或者编译进去都OK，可惜ppa升级又没办法开lua。于是乎，在没有lua的情况下使用neocomplete就会崩溃的发现插入模式下每按一次enter，你就会得到

E117: 未定义的函数: neocomplcache\#enable

这样的错误。接着就要连按几下enter等待这个错误提示结束，然后光标的位置就会多出一个0来。忍了两天终于忍无可忍，于是乎怒了重新装带有lua的vim 7.4。于是乎有了这篇文章。

OK，吐槽结束，转入这个正题，在ubuntu12.04上装带有lua的vim 7.4教程：


```bash
sudo apt-get remove vim vim-runtime vim-gnome vim-tiny vim-common vim-gui-common  
sudo apt-get purge vim vim-runtime vim-gnome vim-tiny vim-common vim-gui-common  
sudo apt-get build-dep vim-gnome  
sudo apt-get install luajit libluajit-5.1 libncurses5-dev libgnome2-dev libgnomeui-dev libgtk2.0-dev libatk1.0-dev libbonoboui2-dev libcairo2-dev libx11-dev libxpm-dev libxt-dev python-dev ruby-dev mercurial  
cd /usr/bin
sudo ln -s luajit-2.0.0-beta9 luajit  
cd ~  
hg clone https://code.google.com/p/vim/ #若google被墙,则可以clone我github上的备份 https://github.com/mirsking/vim.git
cd vim

./configure --with-features=huge --enable-cscope --enable-rubyinterp --enable-largefile --disable-netbeans --enable-pythoninterp --with-python-config-dir=/usr/lib/python2.7/config --enable-perlinterp --enable-luainterp --with-luajit --enable-fail-if-missing --with-lua-prefix=/usr --enable-gui=gnome2 --enable-cscope --prefix=/usr

make VIMRUNTIMEDIR=/usr/share/vim/vim74  
sudo make install

```

命令很简单，卸载，依赖处理，装依赖，编译vim7.4（加入lua参数）。终于可以爽爽的用vim了。



## about vimrc and plugins, 2015-02-08
vim的vimrc一直用的[tianjun](tianjun.ml)学长的, 非常的好用, 不过后来自己添加了一些其他插件, 然后上传到了git上备份. 这里总结一下添加的插件及插件安装方法.

### 插件安装
使用bundle管理插件, 所以首先要下载bundle:

```bash
mkdir -p ~/.vim/bundle/
cd ~/.vim/bundle/
git clone https://github.com/gmarik/vundle.git 

```

接着把.vimrc放到~目录中, 然后启动vim, 这是会报一堆错, 都是因为插件没安装的原因, 一路回车进入vim界面, 然后

```bash
:BundleInstall
```

这样.vimrc中的插件就会自动下载并安装, github网速略慢, 所以这个会持续略久...

### 使用的插件
* YouCompleteMe	: 语义级自动补全神器
* xml.vim :	xml高亮
* syntastic	:	语法检查
* vim-instant-markdown : vim写markdown, 浏览器实时预览
* vim-markdown :	这个也是个markdown插件, 貌似安装比较简单, 不过还没试验过.
