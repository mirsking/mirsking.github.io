---
layout: post
title:  "wordpress后台找回google authenticator验证码"
date: 2014-02-07 09:52 +0800
categories:  工具配置
---

Wordpress中安装google
authenticator插件，可以极大地增强wordpress的后台安全性。该插件的安装及设置，google一下就可以找到一大片，所以笔者在这里就不再累赘。

而本文所要解决的问题则是：如果在手机上误删了google身份验证器，或者其他刷机等原因导致无法获取google
authenticator的验证码而无法登录wordpress后台怎么办？

笔者这两天手残电脑、手机同时刷了系统，无奈无法获得验证码而失去了本博客的后台。Google之却未曾找到解决方案。于是乎发挥自己的聪明才智，找到了简单的解决方案。闲话休提，下面笔者将此方案介绍给大家，以造福互联网。


首先，我们虽然没有了wordpress后台的进入权限，但是我们有数据库的权限，所以登入wordpreess数据库的后台。然后，找到数据库中的wp\_usermeta表，在其中，我们可以发现大量关于google
authenticator的字段：如图


![googleauthenticator](http://cdn.mirsking.com/tools_configuration/googleauthenticator.bmp)

其中googleauthenticator\_description和googleauthenticator\_secret是我们这次要用到的字段。他们就是手机端google身份验证器所需要的帐号和密码。在手机上输入完成后即可得到自己想要的验证码了。

## PS.

当时看到googleauthenticator\_enabled这个字段，猜测如果把它的值由enabled改为disabled是否就直接可以免去验证码这一步。但是试了上文的方法后，就没再尝试。有兴趣的朋友可以一试。


## 总结一下

总之，wordpress所有内容都保存在数据库中，所以只要出现问题，到数据库中一定可以找到解决方案，当然前提是保存好你的数据库登录口令，如果连这个也丢掉了，那就呜呼哀哉了....

