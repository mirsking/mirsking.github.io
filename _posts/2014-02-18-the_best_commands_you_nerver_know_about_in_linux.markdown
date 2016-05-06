---
layout: post
title:  "你绝不知道的Linux中极好用的命令"
date: 2014-02-05 10:03 +0800
categories: Linux 
---

## 引子 

最近发现了一个很好玩的网站Quora。

该网站的立意是：to share and grow the world's knowledge.

而其实现途径则是：Ask any question, get real answers from people with
first hand experience, and blog about what you know.

也就是说任何人可以发布自己的问题，然后由一群高智商、丰富经验的网友来答疑解难从而助力知识大爆发。

听起来类似百度知道，但上边的问题和解答，个人感觉比百度知道的水平要高许多。

## 正文 

好了，闲话休题。下边来为大家介绍在上面看到的一个好帖：《[The Best Commands You Never Knew About in Linux](http://www.quora.com/Jordan-Goldman-4/Posts/The-Best-Commands-You-Never-Knew-About-in-Linux?srid=toPT&share=1 "The Best Commands You Never Knew About in Linux")》。所以将其转过来，帮助自己提高，也推荐给大家。

大家不得不承认，即使是最好的系统管理员也可能从未听说过一到两个好用的命令。下边这些命令，大家可以试一试，我保证他们会节约你的时间，至少会让你感到这些命令好酷！

column——效果是把输入排列成多列。

试一试：mount | column –t

dstat——系统集成检测。快速地告诉你CPU、硬盘、带宽的使用情况等等。

试一试：dstat -a

tac——cat命令的逆操作。将输出保存成一个文件。

试一试：tac \<文件名\>

ctrl + r——查询你使用过的命令历史。如果你忘记了曾经精心输入的长命令，那么试试ctrl+r吧。

watch——在y段时间内，没间隔x秒执行一个命令。

试一试：watch –interval 3 df –h

!\$——存储上一个命令的最后一个参数的值。

试一试：cat \<文件名\>|echo !\$

htop——交互的进程查看器（用来替代top），如果你想杀掉进程列表中的某个进程，那么htop值得一试。

diff3——或许你用过diff命令，但是你曾经需要比较三个文件的不同吗？试试这个命令吧。

cal——在命令窗中工作了许久，想快速看下日历？输入cal看看发生了什么把。

## 结语 

文中提到了一些很奇怪的命令，正因为奇进而我们可能见到的机会比较小。但是有一部分命令如果熟练用起来还是不错的，比如：ctrl+r，比如tac。当然文中列出的命令只是linux庞大命令库的凤毛麟角，欢迎大家回帖分享大家喜欢的命令！
