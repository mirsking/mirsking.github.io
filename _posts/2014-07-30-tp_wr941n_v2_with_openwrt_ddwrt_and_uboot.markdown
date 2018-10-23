---
layout: post
title:  "TP WR941N v2原厂、OpenWRT、DD-WRT、刷不死的Uboot"
date: 2014-07-30 00:15 +0800
categories:  工具配置
---

曾经学长Tianjun告诉我一个好玩的WRTnode，可惜148大洋一个裸板太贵，而且错过了公测…

而家里正好无线网需要中继，于是冲着AR9314大厂的芯入手了一款改版的WR941N，TP的产品向来是早期版本做工扎实稳定，新的版本就不断偷工减料降低成本，所以选择了v2版（为了省事，直接入手的改版机，改了SMA，16M Flash，64M RAM，加USB，基本够折腾了）。

既然有WRTnode的前奏，当然是想刷OpenWRT耍耍，可惜改版机入手后就已经刷成了OpenWRT+刷不死的Uboot。不过回来后发现有一天左右死机一次的bug，于是乎想刷回官方固件，这其中刷坏了Uboot，各种辛酸泪啊。

心酸不表，本文主要总结下刷机历程，以备日后有时间再折腾而翻阅。


## 固件互刷
不用Uboot，最安全的原厂、OpenWRT、DD-WRT互刷。此法乃网络上搞到的，只做简单总结：  

###  原厂——\> OpenWRT

A、直接网页升级固件：openwrt-ar71xx-tl-wr941nd-v2-squashfs-factory.bin。  
[固件下载地址](http://downloads.openwrt.org/snapshots/trunk/ar71xx)  

B、然后telnet到路由，将openwrt-ar71xx-tl-wr941nd-v2-squashfs-sysupgrad.bin固件上传到/tmp目录，使用命令`mtd –r write openwrt-ar71xx-tl-wr941nd-v2-squashfs-sysupgrad.bin firmware` 刷更新包。  

备注：上传文件到路由有两种方法，一种直接用winscp，一种用HFS建立虚拟文件服务器，然后在路由端使用wget命令。

### OpenWRT ——\> DD-WRT  

在Openwrt网页升级factory-to-ddwrt.bin固件，需要的再在DD网页升级tl-wr941nd-webflash.bin固件。  
[固件下载地址](ftp://ftp.dd-wrt.com/others/eko/BrainSlayer-V24-preSP2/2013/04-15-2013-r21286/)  

###  DD-WRT ——\> 原厂  

这个是是用一款神器ddwrt2factory.exe，使用的固件是TP官网下的。不过据我测试，TP13年的那个固件不成功（会不断重启），推荐10年和11年的固件。  
固件下载地址：TP官网



## 刷不死的Uboot  
刷Uboot有风险，提前提醒大家。我的就纠结了好久才搞好。  

### Uboot固件
首先上Uboot固件，941n v2的Uboot在网上能找到很多，但真正能用的不多，经本人测试，推荐这个Uboot（[百度网盘](http://pan.baidu.com/s/1i3sXYGd)）。不过这个Uboot在刷之前，切记将其中的MAC和PIN码修改成自己的，否则无法使用无线网络，切记切记，笔者就是在此地被坑了许久。

修改方法：  

- 用winHex打开这个bin文件，ALT+G转到01fc00处，这里即为MAC地址，默认是112233445566。  

- 同样转到01fe00处，这里修改PIN码，默认是123456

### 刷Uboot
刷Uboot的方法有很多，本文不表编程器法，只讲在telnet或ssh到路由上的方法。  

一般刷Uboot都是在DDWRT下刷，因为OpenWRT对Uboot和ART区域进行了锁定。但是笔者遇到的一个坑爹的情况就是由于刷了未改MAC的Uboot导致刷到OpenWRT后无法返回DD。幸而有haxc大神编译了未锁定Uboot版OpenWRT，于是乎救回了差点变砖的941。OK，本文推荐的haxc编译的可刷Uboot的OpenWRT固件闪亮登场（[百度网盘](http://pan.baidu.com/s/1c06FzjU)）。  

无论是OpenWRT还是DDWRT，刷Uboot的方法都一致。

A、 telnet或ssh到路由  
B、 使用 cat /proc/mtd命令查看mtd情况  
OpenWRT一般是这样的

![uboot](http://cdn.mirsking.com/tools_configuration/uboot.png)

DD-WRT类似，只是最后一列的name一般有所差别，这里的name一定要确定。下一步要用。  

C、 使用命令  

OpenWRT用：`mtd –r write uboot.bin u-boot`  
DDWRT用：`mtd –r write uboot.bin RedBoot` 


其中uboot.bin是uboot的二进制文件。后边的参数就是上一步记下的name，一般OpenWRT是u-boot，而DDWRT是RedBoot。

OK，总结完毕，希望对使用这款路由的机油们有所帮助，更重要的是日后再玩起这个，不会太浪费时间去重新摸索。
