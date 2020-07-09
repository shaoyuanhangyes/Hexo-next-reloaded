---
title: Hexo自定义配置
date: 2019-01-11 19:37:00
categories:
- Hexo配置
tags:
- Hexo
---
### 创建分类页面
添加一个 分类 页面，并在菜单中显示页面链接。
* 1.新建一个页面，命名为 categories 。命令如下：
```bash
$ hexo new page categories
```
* 2.编辑刚新建的页面 index.md，将页面的类型设置为 categories ，主题将自动为这个页面显示所有分类。
```bash
title: categories
date: 2018-01-14 20:26:39
type: "categories"
```
<!-- more --> 
* 3.在菜单中添加链接。编辑主题的 _config.yml ，将 menu 中的 categories: /categories 注释去掉，如下
```bash
menu:
  home: / || home
  #about: /about/ || user
  tags: /tags/ || tags
  categories: /categories/ || th
  archives: /archives/ || archive
```
以后更新文章的时候可以在md文件头部加上categories分类名了
### 创建云标签
添加一个标签云页面，并在菜单中显示页面链接。
* 1.新建一个页面，命名为 tags 。命令如下：
```bash
$ hexo new page "tags"
```
* 2.编辑刚新建的页面 index.md，将页面的类型设置为 tags ，主题将自动为这个页面显示标签云
```bash
title: tags
date: 2018-01-14 21:04:12
type: "tags"
```
* 3.在菜单中添加链接。编辑主题的 _config.yml ，添加 tags 到 menu 中，如下:
```bash
menu:
  home: / || home
  #about: /about/ || user
  tags: /tags/ || tags
  categories: /categories/ || th
  archives: /archives/ || archive
```
### 设置头像
编辑主题的 _config.yml，新增字段 avatar， 值设置成头像的链接地址。
```bash
avatar: /images/avatar.jpg
```
其中，头像的链接地址可以是：
完整的互联网 URL，例如：https://storage.googleapis.com/ygoprodeck.com/pics/10443957.jpg
站点内的地址，例如：


	/uploads/avatar.jpg 需要将你的头像图片放置在 站点的 source/uploads/（可能需要新建uploads目录）
或者

	/images/avatar.jpg 需要将你的头像图片放置在 主题的 source/images/ 目录下。
### 设置社交Link&Icon
####Link
   编辑主题的 _config.yml，编辑字段 social，然后添加社交站点名称与地址即可。例如
```bash
social:
  GitHub: https://github.com/shaoyuanhangyes || github
  Google: https://plus.google.com/110657431429268241192 || google
  Weibo: https://weibo.com/u/1349974130 || weibo
```
####Icon
将要替换的ico文件放在主题的

	source/images目录下
并编辑主题的_config.yml，编辑字段 favicon
```bash
favicon:
  small: /images/favicon.ico
  #medium: /images/favicon-32x32-next.png
  #apple_touch_icon: /images/apple-touch-icon-next.png
  #safari_pinned_tab: /images/logo.svg
  #android_manifest: /images/manifest.json
  #ms_browserconfig: /images/browserconfig.xml
```
### 更改代码高亮样式
编辑主题的 _config.yml 添加字段
```bash
highlight_theme: night
```
为黑底样式（个人偏好）
### 关闭页面底部Hexo驱动等标签
编辑主题的_config.yml
```bash
icon: user

  # If not defined, will be used `author` from Hexo main config.
  copyright:
  # -------------------------------------------------------------
  # Hexo link (Powered by Hexo).
  powered: false

  theme:
    # Theme & scheme info link (Theme - NexT.scheme).
    enable: false
    # Version info of NexT after scheme info (vX.X.X).
    version: false
```
### 设置文章截断（阅读全文）Read More
编辑主题的_config.yml
```bash
auto_excerpt:
  enable: true
  length: 200
```