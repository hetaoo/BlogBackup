---
title: Pages与Hexo搭建个人博客
date: 2017-04-21 17:52:51
tags:
- github
- hexo
- blog
categories: tools
---

{% blockquote %}
  **摘要：** Github Pages与Hexo搭建个人博客的教程，网上一搜一大片，在这简短介绍搭建过程和自己踩得一些坑，主要是为了方便自己以后搭建，不再需要重新去找教程。
{% endblockquote %}
## 为什么选择GitHub Pages？

### 很多人用wordpress，你为什么要用github pages来搭建？
* github pages有300M免费空间，资料自己管理，保存可靠
* 学着用github，享受github的便利，上面有很多大牛，眼界会开阔很多
* 经常逛github，习惯性将一些东西提交到github
* wordpress等需要有自己的虚拟主机等

## 安装软件
* {% link Node.js http://nodejs.org/  %}  

* {% link Git https://git-scm.com/  %}   

{% blockquote %}
  配置github就不说了，有一定经验的开发人员都会配
{% endblockquote %}

## 使用GitHub Pages建立博客
与GitHub建立好链接之后，就可以方便的使用它提供的Pages服务，GitHub Pages分两种，一种是你的GitHub用户名建立的username.github.io这样的用户&组织页（站），另一种是依附项目的pages。
想建立个人博客是用的第一种，形如hetaoo.github.io这样的可访问的站，每个用户名下面只能建立一个。
### github上建立仓库
登录后系统，在github首页，点击页面右下角「New Repository」
{% asset_img hetaoo.github.io.jpg  hetaoo.github.io image %}  
点击「Create Repository」 完成创建。   
接下来开启gh-pages功能，点击界面右侧的Settings，你将会打开这个库的setting页面，向下拖动，直到看见GitHub Pages，如图：   
{% asset_img githubPages.jpg  githubPages image %}   
点击Automatic page generator，Github将会自动替你创建出一个gh-pages的页面。 如果你的配置没有问题，那么大约15分钟之后，yourname.github.io这个网址就可以正常访问了~ 如果yourname.github.io已经可以正常访问了，那么Github一侧的配置已经全部结束了。--我的是已经配置好了的。

## 安装hexo
在命令行中输入：
{% codeblock %}
  npm install hexo-cli -g   
  npm install hexo --save
  hexo -v
{% endcodeblock %}

### hexo的相关配置
接着上面的操作，输入：
{% codeblock %}
  hexo init  
  npm install
{% endcodeblock %}
之后npm将会自动安装你需要的组件，只需要等待npm操作即可。   

#### 首次体验Hexo
继续操作，同样是在命令行中，输入：
{% codeblock %}
  hexo g
  hexo s
{% endcodeblock %}
在浏览器中打开http://localhost:4000/，你将会看到预览效果。

## 怎样将Hexo与github page 联系起来
大概分为以下几步：
* 配置git个人信息
* 配置Deployment

### 配置Git个人信息
如果你之前已经配置好git个人信息，请跳过这一个 步骤，直接来到

1. 设置Git的user name和email：(如果是第一次的话)
{% codeblock %}
  git config --global user.name "hetaoo"
  git config --global user.email "i@hetaoo.cn"
{% endcodeblock %}
2. 生成密钥
{% codeblock %}
  ssh-keygen -t rsa -C "i@hetaoo.cn"
{% endcodeblock %}

### 配置Deployment
同样在_config.yml文件中，找到Deployment，然后按照如下修改：
{% codeblock %}
deploy:
  type: git
  repo: git@github.com:yourname/yourname.github.io.git
  branch: master
{% endcodeblock %}

**踩坑提醒：**
* 注意需要提前安装一个扩展：
{% codeblock %}
  npm install hexo-deployer-git --save
{% endcodeblock %}
如果没有执行者行命令，将会提醒
{% blockquote %}
  deloyer not found:git
{% endblockquote %}
* 如果出现下面这样的错误
{% blockquote %}
Permission denied (publickey).
fatal: Could not read from remote repository.
Please make sure you have the correct access rights
and the repository exists.
{% endblockquote %}
则是因为没有设置好public key所致。

## 主题推荐
每个不同的主题会需要不同的配置，主题配置文件在主题目录下的_config.yml。有两个比较好的主题推荐给大家。
* Yilia
* NexT

具体配置就不写了，很简单。

## 添加插件
添加sitemap和feed插件   
切换到你本地的hexo 目录，在命令行窗口，输入以下命令
{% codeblock %}
  npm install hexo-generator-feed -save
  npm install hexo-generator-sitemap -save
{% endcodeblock %}   
修改_config.yml，增加以下内容
{% codeblock lang:nodejs %}
# Extensions
Plugins:
  - hexo-generator-feed
  - hexo-generator-sitemap
#Feed Atom
feed:
  type: atom
  path: atom.xml
  limit: 20
#sitemap
sitemap:
  path: sitemap.xml
{% endcodeblock %}  
再执行以下命令，部署服务端
{% codeblock %}
  hexo d -g
{% endcodeblock %}

## 添加404 页面
GitHub Pages有提供制作404页面的指引：{% link Custom 404 Pages https://help.github.com/articles/creating-a-custom-404-page-for-your-github-pages-site/  %}
直接在根目录下创建自己的404.html或者404.md就可以。但是自定义404页面仅对绑定顶级域名的项目才起作用，GitHub默认分配的二级域名是不起作用的，使用hexo server在本机调试也是不起作用的。

推荐使用{% link 腾讯公益404  http://www.qq.com/404/  %}

我的404页面配置如下：
{% codeblock lang:html %}
<html>
<head>
<meta http-equiv="content-type" content="text/html;charset=utf-8;"/>
<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" />
<meta name="robots" content="all" />
<meta name="robots" content="index,follow"/>
</head>
<body>

<script type="text/javascript" src="//qzonestyle.gtimg.cn/qzone/hybrid/app/404/search_children.js"
      charset="utf-8" homePageUrl="/"
      homePageName="回到我的主页">
</script>

</body>
</html>
{% endcodeblock %}

## 绑定个人域名
* 在你的source目录下新建CNAME文件，内容写你的域名
* 在你的域名cdn解析中添加cname解析，指向hetaoo.github.io
* 也可添加A解析，ip地址为  

{% asset_img ip.jpg  ip image %}  

## 国内加速
* 开通对象储存oss
* 开通cdn

### oss
新建Bucket   
{% asset_img oss.jpg  oss image %}  
* 储存内容为：低频访问
* 读写权限为公共度

#### oss配置
* 默认首页为：index.html
* 回源规则设置为：镜像-->回源地址：hetaoo.github.io

### cdn
* 添加加速域名   
{% asset_img createCDN.jpg  createCDN image %}

* 添加cname解析才会生效   
{% asset_img cname.jpg  cname image %}


## 最后附上我的_config.yml

{% codeblock %}
# Hexo Configuration
## Docs: https://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site
title: Hetaoo's Blog
subtitle: 简单，美好，奋发
description: 一个简单喜欢编程的程序员
author:  Hetaoo
email: i@hetaoo.cn
keywords: "后端,java,开发者,程序员,程序猿,程序媛,极客,编程,代码,开源,IT网站,Developer,Programmer,Coder"
language: zh-CN

# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://www.hetaoo.cn
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:

# Directory
source_dir: source
public_dir: public
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:

# Writing
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link: true # Open external links in new tab
filename_case: 0
render_drafts: false
post_asset_folder: true
relative_link: false
future: true
highlight:
  enable: true
  line_number: true
  auto_detect: false
  tab_replace:

# Category & Tag
default_category: uncategorized
category_map:
tag_map:

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss

# Pagination
## Set per_page to 0 to disable pagination
per_page: 8
pagination_dir: page

# Archives
## 2: Enable pagination
## 1: Disable pagination
## 0: Fully Disable
archive: 1
category: 1
tag: 1

# Extensions
## Plugins: https://hexo.io/plugins/
plugins: hexo-generator-feed

## Themes: https://hexo.io/themes/
theme: hexo-theme-yilia.git

# Disqus #社会化评论disqus，我使用多说，在主题中配置
disqus_shortname:

sitemap:
    path: sitemap.xml
baidusitemap:
    path: baidusitemap.xm

feed:
    type: atom
    path: atom.xml
    limit: 100

# Markdown
## https://github.com/chjj/marked
markdown:
  gfm: true
  pedantic: false
  sanitize: false
  tables: true
  breaks: true
  smartLists: true
  smartypants: true

# Stylus
stylus:
  compress: false

# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type:git
  branch:master
  repo:https://github.com/hetaoo/hetaoo.github.io.git

jsonContent:
    meta: false
    pages: false
    posts:
      title: true
      date: true
      path: true
      text: true
      raw: false
      content: false
      slug: false
      updated: false
      comments: false
      link: false
      permalink: false
      excerpt: false
      categories: false
      tags: true

{% endcodeblock %}
