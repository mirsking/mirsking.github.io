---
layout: post
title:  "国内源集合（Linux/Python/Ruby/JS）"
date:   2016-04-10 17:00:00 +0800
categories: 工具配置
tags:
- 工具配置
---

## Linux
* [浙大源](http://mirrors.zju.edu.cn)
* [中科大](http://mirrors.ustc.edu.cn/)
* [阿里云](http://mirrors.aliyun.com/)

## Python
* Windows
新建文件%HOMEPATH%/pip/pip.ini，添加如下内容（HOMEPATH就是当前用户目录）：  

```
[global]
index-url = http://mirrors.aliyun.com/pypi/simple
trusted-host = mirrors.aliyun.com
```

* Linux
新建文件~/.pip/pip.conf，添加如下内容：    

```
[global]
index-url = http://mirrors.aliyun.com/pypi/simple
trusted-host = mirrors.aliyun.com
```

## Ruby
本来一直使用淘宝的镜像源ruby.taobao.org，后来听说了这个  
![ruby.taobao.org_unamended@2016410](http://7xnluw.com1.z0.glb.clouddn.com/tools_configuration/ruby.taobao.org_unamended.png)   
所以现在改用[ruby-china的镜像](https://gems.ruby-china.org/)：  

### 对于直接gem安装镜像配置方法

```
gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/
```

配置之后可以用`gem sources -l`来查看当前使用的源

### 对于使用bundle+Gemfile进行包管理的方法
使用

```
bundle config mirror.https://rubygems.org https://gems.ruby-china.org
```

这样就可以不用在在Gemfile里边改镜像源了。

## JS/CSS
[bootstrap中文网](http://www.bootcdn.cn)  
