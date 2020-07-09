---
title: Hello World
date: 2019-01-10 19:37:00
categories: 
- Hexo配置
tags:
- Hexo
---
Windows下Hexo+Github搭建自己的Blog

## 配置环境
github账户：shaoyuanhangyes@outlook.com
建立好名字为shaoyuanhangyes.github.io的仓库并开启Github.Pages

### 安装Node.js
安装完毕后在cmd里输入
``` bash
node -v
npm -v
```
出现版本信息后则说明安装正确
### 安装Git
安装完毕后在cmd里输入
``` bash
git --version
```
出现版本信息后则说明安装正确
<!-- more --> 
### 配置SSH Key

``` bash
$ git init
$ git config --global user.name "shaoyuanhangyes"
$ git config --global user.email "shaoyuanhangyes@outlook.com"
$ ssh-keygen -t rsa -C "shaoyuanhangyes@outlook.com"//连续摁三次空格(设置空密码)
```
在个人文件夹下出现 .shh文件夹 点击进入
用Sublime打开id_rsa.pub文件 复制密钥全部内容
再回到Github的Setting的SSH and GPG keys
点击New SSH key
把复制的信息粘贴到SSH密钥
确认即可

### 安装Hexo
在要安装的文件夹下鼠标右键选择Git Bash Here
``` bash
$ npm install -g hexo-cli
```
敲完回车可能没有任何提示，请一定要耐心等待
安装成功后，可以输入以下命令测试以下Hexo是否安装成功
``` bash
$ hexo version
```
如果能看到hexo的版本号信息，就表示安装成功了
然后依次输入以下命令
``` bash
$ hexo init
$ npm install
$ hexo g
$ hexo s
```
这时候在浏览器输入 http://localhost:4000 就可以看到本地生成了Hexo博客界面
### 配置Hexo到Github
在hexo文件夹下找到_config.yml文件用sublime打开拖到最底部将deploy设置更改为
``` bash
deploy:
  type: git
  repository: git@github.com:shaoyuanhangyes/shaoyuanhangyes.github.io.git
  branch: master
```
保存后在git里敲命令
``` bash
$ hexo g//generate
$ hexo d//deploy
```
若出现以下异常
``` bash
ERROR Deployer not found: git
```
尝试输入以下命令
``` bash
$ npm install hexo-deployer-git --save
$ hexo g
$ hexo d
```
这时候如果弹出一个对话框，输入在guthub上面的用户名和密码即可
这时 访问Github的Page页面就是Hexo搭建的博客页面了
## 更新Blog
### 新建文章
``` bash
$ hexo new [layout] <title>
```
新建一篇文章。如果没有设置 layout 的话，默认使用 _config.yml 中的 default_layout 参数代替。如果标题包含空格的话，请使用引号括起来。
### 生成静态页面
``` bash
$ hexo g
```
### 部署网站
``` bash
$ hexo d
```

## 配置主题为next

### clone next文件
```bash
git clone https://github.com/iissnan/hexo-theme-next themes/next
```
将hexo/_config.yml内主题更改为next
```bash
theme:next
```
### 个性化设置
#### 增加标签 分类 归档菜单 更改主题为Pisces
文件目录hexo/themes/next下_config.yml
```bash
scheme: Pisces
#next主题为Pisces
menu:
  home: / || home
  #about: /about/ || user
  tags: /tags/ || tags
  categories: /categories/ || th
  archives: /archives/ || archive
  #schedule: /schedule/ || calendar
  #sitemap: /sitemap.xml || sitemap
  #commonweal: /404/ || heartbeat
```
#### 更改个人头像 网页icon 社交软件按钮菜单 代码黑底高亮风格
将个人头像avatar.jpg放入hexo/themes/next/source/images
将网页图标favicon.jpg放入hexo/themes/next/source/images(favicon.ico一直不显示 有可能是我采用的图标像素太大 最好是32x32)
文件目录hexo/themes/next下_config.yml
```bash
avatar: /images/avatar.jpg

favicon:
  small: /images/favicon.jpg
  medium: /images/favicon.jpg
  #apple_touch_icon: /images/apple-touch-icon-next.png
  #safari_pinned_tab: /images/logo.svg
  #android_manifest: /images/manifest.json
  #ms_browserconfig: /images/browserconfig.xml

social:
  GitHub: https://github.com/shaoyuanhangyes || github
  E-Mail: mailto:shaoyuanhangoutlook@gmail.com || envelope
  Google: https://plus.google.com/110657431429268241192 || google
  Twitter: https://twitter.com/Ruojhen_syh || twitter
  #FB Page: https://www.facebook.com/yourname || facebook
  #VK Group: https://vk.com/yourname || vk
  #StackOverflow: https://stackoverflow.com/yourname || stack-overflow
  #YouTube: https://youtube.com/yourname || youtube
  Instagram: https://instagram.com/Ruojhen_syh || instagram
  #Skype: skype:yourname?call|chat || skype
  Weibo: https://weibo.com/u/1349974130 || weibo

highlight_theme: night
```
更多个性化设置正在摸索未完待续......