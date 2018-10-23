---
layout: post
title:  "Ubuntu下安装Java插件连接浙大RVPN"
date: 2014-05-25 17:47 +0800
categories:  工具配置
---

浙大的RVPN是通过访问rvpn.zju.edu.cn，然后调用Java插件运行Sangfor(深信服)的client然后添加本地路由表实现的。

windows平台IE上很容易安装Java插件，进而自动下载Sanfor的client。唯一需要注意的是在第一次访问rvpn.zju.edu.cn的时候最好把本地防火墙关闭，否则很可能登陆后一切就绪却卡在“正在初始化”阶段。这个阶段一般是因为权限不够或防火墙禁止了下载客户端、安装vpn虚拟网卡导致的。所以最好是把防火墙关了吧，反正也是鸡肋的东西。

本文主要是记录下Ubuntu下安装Java插件并连接RVPN的过程。下面就开始喽：

## Java插件篇

Ubuntu中常用的浏览器就是firefox和chrome。先说chrome，笔者用惯了chrome的各种优秀插件和同步功能，虽然firefox也有，但是firefox的UI现在都开始抄袭chrome了，这让笔者更坚定了chrome的阵营。

可惜，就是这一坚定，让笔者走了不少弯路。本文发问之日，chrome已经更新到了稳定版35.0；而据官网介绍Chrome35移除了对像Java这类NPAPI插件的支持。所以此路不通，可惜了笔者还尝试了互联网上各种方法那么久。不过倒是有个解决方案就是降级使用chrome 34，这个可以到[Chrome](http://mirror.pcbeta.com/google/chrome/deb/pool/main/g/google-chrome-stable/)这里去下载deb包。

笔者喜欢追新，所以没有选用此法，而是被迫安装了firefox。以下是下载和安装java插件的步骤：

1.  浙大的RVPN在windows下用jre7u55测试通过，不过貌似ubuntu下不行，官网推荐6u27版，可以[点击这里下载](http://www.oracle.com/technetwork/java/javasebusiness/downloads/java-archive-downloads-javase6-419409.html#jre-6u27-oth-JPR)。
2.  下载的bin文件，依次执行
	
	```
	sudo chmod +x jre-6u27-linux-x64.bin  
	./jre-6u27-linux-x64.bin
	```
    即可得到一个jre1.6.0\_27的目录，本文要用到的就是目录中的lib/amd64/libnpjp2.so（x86用户自行把amd64换为i386）

3.  cd到mozilla（firefox）的插件目录，一般是/usr/lib/mozilla/plugins/。然后运行

	```
    sudo ln -s \$JAVA\_HOME/lib/amd64/libnpjp2.so
	```
    注意这里不用设置环境变量`JAVA_HOME`那么麻烦，只要把`$JAVA_HOME`替换为你第二步生成的文件夹目录即可。

4.  然后重启firefox，看下about://plugins中Java插件是否安装成功。

其实chrome的安装步骤类似，只是其插件目录一般是/opt/google/chrome/plugins/，此处的plugins目录如果没有，自行mkdir即可。只可惜35版本的chrome不支持Java插件了...

 

## rvpn的设置

java安装成功后，就可以访问rvpn了。访问rvpn.zju.edu.cn登陆后，会运行一个java插件，同意运行即可。然后会下载sangfor的一个client到`~/.sangfor`目录。这里注意一下打开firefox时最好在terminal中用`sudo firefox`或`gksudo firefoxi`打开，因为sangfor在运行时会虚拟一个tun0的网卡，然后建立一组路由表，这个是需要root权限的。

这里如果运行正常，就会出现

![easyconnect](http://cdn.mirsking.com/tools_configuration/easyconnect.png)

这个小图标。然后就可以尽情访问内网了。

但是这里笔者在执行的时候遇到了一个问题。现象是：登陆显示正常，但是没有绿色的小图标，也就无法访问内网。

笔者当时尝试了n种方法，郁闷到了极点。出去转了一圈，笔者就想，在windows上，java插件运行后既然调用的是sangfor的exe然后出现了绿色图标，也即easyconnect，那么ubuntu一定是调用sangfor这一步出错导致了上述现象。

于是，笔者cd到`~/.sangfor/ssl/`发现了一个shell文件夹，然后发现了sslservice.sh这个脚本，于是运行一下，发现一直报错，如下：
```
error while loading shared libraries: libstdc++.so.6: cannot open shared
object file: No such file or directory
```
ok，共享库的问题，于是google之，笔者找到了解决方法，运行
```
sudo apt-get install libstdc++6  
sudo apt-get install lib32stdc++6
```
安装这个依赖包，再次访问rvpn，一切正常。

## 小结

RVPN在windows下很easy，特别是有IE浏览器这种所有开发必须兼容的强大浏览器。

而Ubuntu下其实Firefox/Chrome也都是支持的，可惜Chrome升级35版后放弃了对Java插件的支持。

RVPN在windows和Ubuntu下倒腾了两天，想来这种东西常常是某人按官网流程来一遍就所有OK，某人搞了N久却一直搞不定的东西。所以这种东西总是不能一味照着别人的步子重复的，还是要搞的时候思路清楚，把握其中的原理，然后借助强大的google，问题就迎刃而解了！
