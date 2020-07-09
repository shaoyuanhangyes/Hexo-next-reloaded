---
title: Windows配置FTP
date: 2020-02-05 10:43:19
categories:
- FTP
tags:
- FTP
- Microsoft Windows
mathjax: true
---
## Windows配置FTP服务器
### Windows下通过IIS配置FTP注意事项
一般用于解决本地能访问FTP目录而同一局域网下的其他设备无法访问的问题
<!-- more --> 
#### 防火墙规则
防火墙的入站出站规则有关于FTP的必须全部启动
新建入站出站规则针对所有本地TCP端口
#### IIS基本设置
在IIS下的网站主页中 点击右边侧栏的基本设置 连接为选择特定用户 测试设置能显示全部通过