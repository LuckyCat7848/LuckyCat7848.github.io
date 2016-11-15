---
layout: post
title:  使用jekyll在GitHub Pages上搭建个人博客
date:   2016-11-15 17:55:01 +0800
categories: 技术
tag: jekyll
---

* TOC
{:toc}





*GitHub Pages* *jekyll* *独立域名* *域名解析*


>网上已经有足够多的相关介绍了，本人也是根据他人写的文章来做的。实践过程中发现很多文章都多少有些过时，提供的操作流程都是旧版网页的，以至于很多地方遇到障碍。还有，很多东西没有详细解释对于小白来说很是不懂。总之，磕磕绊绊的，在此整理下，希望对别人也能有所用处。

>网上有很多搭建博客的方式，如果你不知道如何选择，可以看看[《What are the best solutions for a personal blog?》](http://www.slant.co/topics/329/~what-is-the-best-solution-for-a-personal-blog)，文章分析了不同博客平台的优缺点和针对人群。最终，我的选择是Jekyll+Github Pages。

-------------------

[TOC]


Step 1. 服务器：GitHub
====================================

1.1. 什么是 GitHub？
------------------------------------

在说GitHub之前，必须要提到Git。Git是分布式版本控制系统。GitHub可以托管各种Git版本库，并提供一个web界面。
Github 就像是程序员们的Facebook，程序员们，写代码，做项目，在此和全世界的人们分享。
会使用GitHub的资源，比会搭建个人博客的价值大得多。

1.2. 什么是GitHub Pages？
------------------------------------

>Website for you and your project.

GitHub Pages有两种。一种是为个人或者组织的博客。一种是为项目的博客。前者一个账号只能建一个，后者，可以建很多个。
这样的博客，免费、独立、安全。

1.3. 创建GitHub Pages博客
------------------------------------

- 首先，注册 [Github](https://github.com/)
- 然后，建立一个仓库

>**Repository name(仓库名)必须是 your_user_name.github.io**

比如，我的GitHub用户名是LuckyCat7848，那么仓库名就取为 LuckyCat7848.github.io。这一点很重要，必须这么写！因为GitHub上的用户名是唯一的，所以上面说一个账号只能建一个个人博客。

- 然后，按照提示步骤操作
- 最后，新建一个index.html文件，push。

打开终端，切换到本地GitHub目录下。（本人之前配置过，如果没有操作过的，自己找教程吧。）

克隆GitHub上的这个Responsitory文件夹到本地：

切换到该Responsitory下，并且创建一个只写着Hello World的.html文件：

提交这些改动：

官网上有命令行和客户端两种操作方式（[官网教程](https://pages.github.com/)）。等一小会，网站生效，访问your_user_name.github.io，就能看见完整的网页了，此时网页上还只有一句Hello World。



Step 2. 安装jekyll
====================================

2.1. 什么是 jekyll？
------------------------------------

>[Jekyll](http://jekyllrb.com/) is a simple, blog-aware, static site generator.
[Jekyll](http://jekyllrb.com/) 是一个简单的博客形态的静态站点生产机器。

解释一下，Jekyll可以将纯文本转换为静态博客网站。你整个网站的页面都是它生成的，从主页index到文章post。
比如，文章怎么写？标准网页格式是扩展标记语言HTML。纯手写？未免太麻烦。大家，多偏爱Markdown。所以，就用它写。不过，你需要有一个能把你用Markdown格式写的文章，转化为HTML网页的东西，这里使用的就是静态网页生成器。

2.2. Jekyll目录
------------------------------------

解释一下整个jekyll的目录:
为了之后不至于完全茫然，很值得先看一看，第一次看不懂没关系，用着用着就知道什么意思了。就像练习吉他和弦的转换，开始很难，可换着换着你就会了。

这个很奇怪的结构是，文件目录树，_config.yml这样的代表一个文件，_drafts这样的代表一个文件夹，与它连接的文件，比如begin-with-the-crazy-ideas.textile，就在文件夹中。一开始，我没怎么看官方文档，嫌麻烦，不如直接开始干。结果是绕了不少弯路，修改主题的时候，找半天各个部分。

先说需要了解的，其余以后依个人需求学习。

>- _config.yml 是配置文件，你可以在里面配置你博客会用到的常量，比如博客名，邮件。
>- _includes：就是你文章各个部分的html文件，可以在布局中包含这些文件。
>- _layouts：存放模板。就是你网页的布局，主页布局，文章布局。当然不是指CSS那样的布局，是指，你包含哪些基本的内容到页面上。包含的内容就是includes里面的文件。
>- _posts: 存放博客文章。
>- index：博客主页。
>- CNAME文件：域名地址。
>- CSS：存放博客所用CSS。
>- JS: 存放博客所用JavaScript。

可以设置每个html文件的title(标题)和layout(布局)。比如index的layout一般是default。你也可以添加其他的页面，加上不同的layout。

当你想定制博客的时候，以上目录就很有用了。

2.3. 安装Jekyll
------------------------------------

- 安装Ruby，Mac一般默认安装了Ruby，这一步可以忽略。[官网安装](https://www.ruby-lang.org/zh_cn/downloads/)
- 安装Bundler，在Terminal中输入：

>**sudo gem install bundler**

如果出现以下error：（不好意思，图丢失了）
使用命令行：

>**sudo gem install -n /usr/local/bin bundler**

- 安装Jekyll：

>**sudo gem install jekyll**

如果出现以下error：（不好意思，图丢失了）

使用命令行：

>**sudo gem install -n /usr/local/bin jekyll**

建议翻墙安装，墙内不知是否会出现问题。



Step 3. 装修博客
====================================

3.1. 如何选择和修改主题？
------------------------------------

- 一种方式是使用他人写好的，免费开源的。
>[Jekyll 主题](http://jekyllthemes.org/)

- 另一种是，你也可以自己写或修改，需要懂一些前端的知识。

3.2. 如何使用jekyll博客模板
------------------------------------

- 首先，在[Jekyll 主题](http://jekyllthemes.org/)找到喜欢的模板并下载下来。
- 在终端里切换到your_user_name.github.io目录下，把下载下来的**文件下的全部内容**转移到当前目录下，可以用命令行也可以直接到文件夹下操作，更方便。最终目录如下：

- 输入jekyll命令本地查看博客效果：

>**jekyll serve –watch**

该命令行输入后，jekyll运行起来，就可以在浏览器中输入“http://localhost:4000/”查看网页效果了，终端中按“Ctrl+C”停止运行该命令。

此时，博客上的内容都不是自己的东西。在终端中终止运行jekyll后，编辑该目录下的_config.yml等文件就可以修改网页上的内容了，参考上述jekyll目录内容自己慢慢摸索即可。


Step 4. 独立域名
====================================

4.1. 购买独立域名
------------------------------------

- 购买域名，著名的有[Godaddy](https://uk.godaddy.com/)，支持支付宝。在网上可以搜到优惠码，优惠下来价格不高。
- 搜索自己想要的域名，购买时切记去搜索优惠码。
结算的时候本人是用信用卡支付的，不同的优惠码优惠的产品和优惠多少和支持的支付方式不同，信用卡的话要用VISA全球卡。
还有我遇到过结算的时候怎么都结算不了，也是因为优惠码的问题，多试几个就好了。

4.2. GitHub上的修改
------------------------------------

在终端中或GitHub上在your_user_name.github.io修改CNAME文件，在里面写入你刚刚买的域名，例如我的是“LuckyCatHome.com”。
在终端改的话改完记得向上面那样操作，提交到GitHub上。

4.3. DnsPod域名解析
------------------------------------

[DnsPod](https://www.dnspod.cn/)做域名解析处理可以加快博客的访问。

- 首先，注册DnsPods。
- 添加购买好的域名：

- 设置如下：

>第2、3条是默认设置好的。
第一条的记录值是“your_user_name.github.io.”，io后面还有一个.，有文章特注了，我也没有试过不写的后果，还是写上吧。
最后两条的记录值是GitHub Pages的服务器地址。

作用：

>.com需要使用A记录进行域名解析。
告诉所有DNS服务器，对于LuckyCat.com的访问都会被重定向到LuckyCat7848.github.io。

4.4. Godaddy设置NS
------------------------------------

- 点击该域名箭头下的“Set Nameservers”

- 添加DnsPod的两个默认记录值，如下：

稍等个10分钟左右，在浏览器里输入你的域名就可以看到博客内容啦！

>不要忘记初衷，在折腾之后，表达写作，才是最重要的事情。



