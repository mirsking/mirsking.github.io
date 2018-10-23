---
layout: post
title:  "使用Pelican在Github Pages上写博客"
date:   2015-01-12 21:40+0800
categories:  工具配置
---


2014年的寒假，大四即将结束，天生爱折腾的我买了域名、买了go（坑）daddy（爹）的空间搭建起了个人的博客，整个博客的模块完全仿照[tianjun](tianjun.ml)展开，用的却是大而笨的wordpress。现如今，一年过去了，买的一年的空间就要过期，却发现自己的博客里边只静静地躺着21篇博文(加上草稿)...


目前国内虚拟主机除了阿里云不太敢用，国外主机速度太慢，还经常被Q。想了良久，还是回归一个码农的本原，就在github.io上搭一个静态站吧，主要还是监督自己注意总结自己的学习内容记录下来。


Github上建站大都式用jekyl、octopress，可惜都是用ruby写的。ruby不会也就不容易维护，于是放狗搜了一把有没有python的建站，发现还真有[Pelican](http://docs.getpelican.com/en/3.5.0/index.html),果断用起来，现在把过程记下来，算是2015开博的第一篇吧（虽然很快就要考试了...还是要克服万难，坚持下来！）


### pelican 安装

```bash
pip install pelican 
mkdir blog && cd blog
pelican-quickstart
```
最后一步完成后，在blog目录会生成如下图的目录，以后只要在content目录下写markdown，然后在blog目录下`pelican`一下，静态文件就输出到output文件夹了。

![pelican目录结构](http://cdn.mirsking.com/tools_configuration/150112_pelican_direction_struct.png)

### pelican markdown 支持

```bash
pip install Markdown
```

### pelican 主题的安装与配置

```bash
pelican-themes -l #显示已安装主题
git clone https://github.com/getpelican/pelican-themes.git #clone pelican's themes on github
pelican-themes -i [Theme's path]
```
安装好theme后，`pelican-themes -l`中就可以看到已安装的主题了，然后在pelicanconf.py中添加变量`THEME='theme's name`就可以了。

2015，为我自己加油！

