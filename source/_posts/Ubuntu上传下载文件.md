---
title: Ubuntu上传下载文件
date: 2019-06-14 12:25:40
categories: 
- Linux
tags:
- Linux
---
# 使用pscp上传下载文件

## pscp上传文件到指定目录
eg. 将Windows10下D:\Code\datastructure.txt文件上传到Ubuntu下var/www目录下
cmd的pscp命令为
```bash
cd /D D:\Code\
pscp D:\Code\php.zip root@ip:/var/www/
#ip为服务器的公网ip地址
#然后输入root的密码即可上传成功
```
<!-- more --> 
## pscp下载文件到指定目录
eg. 将Ubuntu下/var/www/目录下所有文件都下载到Windows10下D:\Code目录下
cmd的pscp命令为
```bash
cd /D D:\Code\
pscp -r root@ip:/var/www "D:\Code"
#ip为服务器的公网ip地址
#然后输入root的密码即可下载成功
```