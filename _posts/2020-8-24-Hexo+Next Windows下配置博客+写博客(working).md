---

title: Hexo + Next Windows下配置博客+写博客(working)
date: 2020-8-24 0:00:00
description: 搭建博客的经验与心得
categories:

 - blogs

---


# Hexo + Next Windows下配置博客+写博客(working)
## 一、前言
Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。Github作为现在最流行的代码仓库，已经得到很多大公司和项目的青睐。为使项目更方便的被人理解，介绍页面肯定是少不了，甚至会需要完整的文档站。因此Github提供了Github Pages的服务，不仅可以方便的为项目建立介绍站点，也可以用来建立个人博客。

Github Pages有如下特点

* 轻量，无需麻烦的配置
* 无需自己搭建服务器，每个站有300MB的空间
* 使用Jekyll模板系统，适合静态页发布，但动态部分非常局限
  
对个人博客来说，Github Pages算是最完美的解决方案了
## 二、准备工作
### 1.域名
Github Pages无需购买服务器，但它可以绑定域名。如果您需要购买域名可以访问[namesilo](https://www.namesilo.com/)或是[godaddy](https://sg.godaddy.com/zh)
### 2.准备Github仓库
访问[Github](https://github.com/)创建账号，然后新建一个名为**username.github.io**的仓库，注意勾选**Initialize this repository witha README**，空项目clone到本地后会有诸多不便。

为什么要这么命名？简单来说，GitHub Pages有两种类型：User/Organization Pages 和 Project Pages，而我所使用的是User Pages。这两个的区别如下：

* User Pages 是用来展示用户的，而 Project Pages 是用来展示项目的。
* 用于存放 User Pages 的仓库必须使用username.github.io的命名规则，而 Project Pages 则没有特殊的要求。
* User Pages 将使用仓库的 master 分支，而 Project Pages 将使用 gh-pages 分支。
* User Pages 通过 http(s)://username.github.io 进行访问，而 Projects Pages通过 http(s)://username.github.io/projectname 进行访问。

### 3.准备Git本地环境
Git和Github是两个完全不同的概念。

Git是一个版本管理工具，作用就是可以更好的管理本地程序，Github是一个网站，用来交流学习。

安装Git很简单，新手的话所有选项按照默认的选项安装即可。可访问[Git](https://git-scm.com/)来获取安装包。

Git安装完成后可在Git Bash中进行配置
#### 个人配置
进入Git Bash后先配置个人信息，XXXXX部分别是你Github的用户名和注册的邮箱。
``` bash
git config --global user.name "XXXXX"
git config --global user.email "XXXXX"
```
#### SSH key
我们如果要提交代码的话需要权限，但是一直通过验证用户名和密码的方式过于危险。所以我们采取SSH key来解决本地和服务器的连接问题。

可通过如下命令检查是否有SSH Key。
``` bash
ls -al ~/.ssh
```
如果没有的话就通过如下命令生成。
``` bash
ssh-keygen -t rsa -C "your_email@example.com"
```
连续默认即可，最后在对应生成目录下找到id_rsa.pub，复制其中的内容后在 setting -> SSH and GPG keys -> New SSH key设置即可。最后通过如下命令测试
``` bash
ssh -T git@github.com
```
若有Hi XXX的提示后意味着配置成功。

目前的配置就是这些。
### 4.准备Node.js
我是直接在官网上安装，其他方法可能也会陆续更新：[Node.js](http://nodejs.cn/download/)

## 三、Hexo配置
Hexo配置可参考[Hexo文档](https://hexo.io/zh-cn/docs/)
上述准备工作做好之后我们可以正式安装Hexo，非常简单。
### 1.安装
``` powershell
npm install -g hexo-cli
```
### 2.初始化
``` bash
hexo init + 需要初始化的目录
```
这里建议init与你github仓库一样的名字.
### 3.安装依赖
进入初始化的目录后，执行
``` bash
npm install
```
### 4.生成静态页面
``` bash
hexo g
```
### 5.启动本地服务
``` bash
hexo server
```
### 6.测试
访问http://localhost:4000，若无报错即安装完毕。
### 7.使用Next主题
首先，复制一份打开本地博客目录下的 _config.yml文件，命名为 _config_bak.yml，做为备份。
其次安装Next主题
``` bash
 git clone https://github.com/theme-next/hexo-theme-next themes/next-reloaded
```
而后在_config.yml中设置主题
``` yml
theme: next-reloaded
```

**注意，每次切换主题后，验证主题前都要清楚Hexo的缓存**
``` bash
hexo clean
```
验证方法同上。
### 8.Next详细配置文件
```yml
# Hexo Configuration
## Docs: https://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/
## 设置项的键值之间一定要有空格

# Site
title: 乌龙茶馆    # 网站标题
subtitle: 'PWN手，老HAM(BG2DXD/KI5GSH)，技术宅，极客，数学'   # 网站副标题
description: 'About Life, Books and Code.'   # 网站描述 主要用于SEO
keywords: "Rust，C，python，Matlab，Verilog HDL"   # 作者名字 用于主题显示文章的作者
author: Adam Zhang       # 作者名字 用于主题显示文章的作者
language: zh-CN        # 网站使用的语言
timezone: ''     # 网站时区 Hexo 默认使用您电脑的时区

# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://yoursite.com   
## 如果您的网站存放在子目录中，例如 http://yoursite.com/blog，则请将您的 url 设为 http://yoursite.com/blog 并把 root 设为 /blog/
## 这项暂时不需要配置，绑定域名后，要创建 sitemap.xml 时再配置该项
root: /   # 网站根目录
permalink: :year/:month/:day/:title/  # 文章的永久链接格式
permalink_defaults:     # 永久链接中各部分的默认值
pretty_urls:
  trailing_index: true # Set to false to remove trailing 'index.html' from permalinks
  trailing_html: true # Set to false to remove trailing '.html' from permalinks

# Directory
# 目录，如果您刚刚开始接触Hexo，通常没有必要修改这一部分的值
source_dir: source   # 资源文件夹，这个文件夹用来存放内容
public_dir: public   # 公共文件夹，这个文件夹用于存放生成的站点文件
tag_dir: tags       # 标签文件夹
archive_dir: archives     # 归档文件夹
category_dir: categories   # 分类文件夹
code_dir: downloads/code   # Include code 文件夹
i18n_dir: :lang          # 国际化（i18n）文件夹
skip_render:            # 跳过指定文件的渲染，您可使用 glob 表达式来匹配路径
​
# Writing
# 文章布局、写作格式的定义，不建议修改
new_post_name: :title.md  # File name of new posts 新文章的文件名称
default_layout: post   # 预设布局
titlecase: false # Transform title into titlecase  把标题转换为 title case
external_link: 
  enable: true # Open external links in new tab  在新标签中打开链接
  field: site # Apply to the whole site
  exclude: '' 
filename_case: 0    # 把文件名称转换为 (1) 小写或 (2) 大写
render_drafts: false  # 显示草稿
post_asset_folder: false   # 启动 Asset 文件夹
relative_link: false      # 把链接改为与根目录的相对位址
future: true             # 显示未来的文章
highlight:            # 代码块的设置
  enable: true
  line_number: true
  auto_detect: false
  tab_replace: ''
  wrap: true
  hljs: false
prismjs:
  enable: false
  preprocess: true
  line_number: true
  tab_replace: ''

# Home page setting
# path: Root path for your blogs index page. (default = '')
# per_page: Posts displayed per page. (0 = disable pagination)
# order_by: Posts order. (Order by date descending by default)
index_generator:
  path: ''
  per_page: 10
  order_by: -date

# Category & Tag
default_category: uncategorized     # 默认分类
category_map:                      # 分类别名
tag_map:                        # 标签别名

# Metadata elements
## https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta
meta_generator: true

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD    # 日期格式
time_format: HH:mm:ss      # 时间格式
## updated_option supports 'mtime', 'date', 'empty'
updated_option: 'mtime'

# Pagination
## Set per_page to 0 to disable pagination
per_page: 10   # 每页显示的文章量 (0 = 关闭分页功能)
pagination_dir: page   # 分页目录

# Include / Exclude file(s)
## include:/exclude: options only apply to the 'source/' folder
include:
exclude:
ignore:

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next-reloaded  # 当前主题名称，值为false时禁用主题
 
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
## 部署部分的设置
deploy:
  type: ''
```
