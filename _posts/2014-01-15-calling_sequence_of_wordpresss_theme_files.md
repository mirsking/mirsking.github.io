---
layout: post
title:  "WordPress主题文件的调用顺序"
date: 2014-01-15 08:10 +0800
categories:  工具配置
---

本文将展示主题文件执行的层次结构。简而言之,当你加载一个页面时，我们要看看哪些模板文件被执行。您可能已经知道单篇日志是用single.php解析的。单个页面是加载page.php来render的。但WordPress将根据各种实际情况来寻找不同的模板文件,因此，这里，我们就要看看这是如何工作的。

我们首先应该清楚这一点:没有 index.php和style.css你的主题已经不再是一个有效的WP主题…所以理所当然,如果你只有这两个文件,每一个网页被render时WP都将试图加载index.php。各位客观且快速浏览一眼下面这个“cheatsheet”来看看我所指的:  

[![TemplateMap](http://www.yeahzan.com/zanblog/wp-content/uploads/2013/07/TemplateMap.jpg)](http://www.yeahzan.com/zanblog/wp-content/uploads/2013/07/TemplateMap.jpg)  
（ps:这个图应该从上至下，从左往右看。）

注意一下每个页面类型的执行流程都将在index.php终结。这就是为什么index.php是每一个WordPress主题所必须的文件。如果我们在WP主题中遗漏任何其他文件(例如,如果没有“search.php”),WP会自动调用index.php。

现在让我们来看看一些执行顺序的细节问题。  
我要向你们展示WP在你当前活动主题文件夹中搜索文件的流程。  
当你打算从现在开始创造一个WordPress主题的话，我希望这将会是有用的:  
我将会向你展示每一类型的文件执行的层次。

------------------------------------------------------------------------

### 首页

对于每一个网站，这是第一,也是最重要的一个页面。所以WP提供了极大的范围来让你定制这个页面。让我们看看这个用于显示首页的文件的层次。

> 1.  front-page.php
> 2.  home.php
> 3.  index.php

当客户端请求主页时,WP将搜索front-page.php。
如果不存在,它将会使用home.php。如果
home.php存在,它会用到它。否则,它会默认采用index.php。

### 单篇日志

> 1.  single-**[post-type]**.php
> 2.  single.php
> 3.  index.php

只要你需要，WordPress可以有各种日志类型。这将会更容易使得所有/一些日志类型可以有不同的设计。默认情况下“post”是WP主要和默认的日志类型。

这样，举例来说,如果你的自定义日志类型是 product
,那么它的模板将会是single-product.php

了解更多如何添加新日志类型,你可以参考[这个链接](http://ihacklog.com/l.php?url=http%3A%2F%2Fcodex.wordpress.org%2FFunction_Reference%2Fregister_post_type)。

### 单个静态页面

其实这个在WP里面就叫page,这里翻译成中文后反倒不好分清了。

> 1.  **[custom-template]**.php
> 2.  page-**[slug]**.php
> 3.  page-**[id]**.php
> 4.  page.php
> 5.  index.php

与post类型一样,类型,我们可以使用自定义页模板让page类型的页面有不同的页面布局。WP首先搜索指定的页面模板文件(如果存在)。

如果没有找到,它将寻找带有当前页面别名(slug)的模板文件。基本上,如果别名是aboutus,那么它将在当前主题文件夹中搜索文件page-aboutus.php。

WP将像搜索别名页面模板一样搜索文件ID模板。

### 分类

> 1.  category-**[slug]**.php
> 2.  category-**[id]**.php
> 3.  category.php
> 4.  archive.php
> 5.  index.php

我相信，如果你已经看完了上面的话，这里应该不用我解释了。文件搜索规则是一样的。

### 标签

> 1.  tag-**[slug]**.php
> 2.  tag-**[id]**.php
> 3.  tag.php
> 4.  archive.php
> 5.  index.php

### 其它分类（Taxonomy）

> 1.  taxonomy-**[tax]**-**[term]**.php
> 2.  taxonomy-**[tax]**.php
> 3.  taxonomy.php
> 4.  archive.php
> 5.  index.php

这里原文并没有做多少解释。但个人觉得这里要稍微解释下。什么是taxonomy?它的英文意思很简单，就是“分类”。但是在WP里面仅这么说的话，我相应很多人还是会一头雾水。还有，什么是term
? term 的英文意思是术语。  

在WP里面，term可以是post\_tag(日志标签)、link\_category（链接类别）、category(日志分类）及任何其它自定义的分类。例如，自定义了一个名为book(书籍，自定义日志类型）的日志类型，可以把
writer(作家)作为taxonomy ,那么作家的名字，如 hanhan
(韩寒），就是term之一，一个taxonomy下可以有很多term.也就是说，taxonomy是term的一个集合。这样，我们就可以有taxonomy-writer-hanhan.php
作为显示韩寒的书籍分类页面的模板，taxonomy-writer.php
作为显示书籍分类的模板。

### 作者

> 1.  author-**[author-nicname]**.php
> 2.  author-**[author-id]**.php
> 3.  author.php
> 4.  archive.php
> 5.  index.php

### 附件

> 1.  **[mime-type]**.php
> 2.  attachment.php
> 3.  single.php
> 4.  index.php

### 日期

> 1.  date.php
> 2.  archive.php
> 3.  index.php

### 存档

> 1.  archive.php
> 2.  index.php

### 搜索

> 1.  search.php
> 2.  index.php

搜索模板用于显示搜索结果。

### 404页面

> 1.  404.php
> 2.  index.php

### 结论

显然你可以使用这些知识在广泛的方法不同为各种类型的页面加载自定义模板…在很多情况下,即使你在使用一个现有的主题,你仍可以在不改变现有模板文件的情况下得到一个定制的解决方案。你只是需要创建新文件,并按照上述规则指定一个新的名字。

分享你的想法和任何可以包含以上的层次结构的附加文件。

------------------------------------------------------------------------

### 后话

也话有朋友会说，你这个文章中列的文件不完整，不是还有 comments.php 和
comments-popup.php
吗？是的，对于一个标准的主题，这是应该有的。不过，本文不是在讨论WP主题应该有哪些文件，而是讨论主题文件的执行顺序问题，归根结底，comments.php
和 comments-popup.php
不是被WP直接调用的，而是由主题制作者自行调用的（由single.php或page.php调用**\<?php
comments\_template(); ?\>**）。因此，原作者在这里没有列出comments.php 和
comments-popup.php 我想也是完全合理的。

本文转载自  ihacklog.com
