---
title: Git上传文件到仓库中
date: 2019-01-12 19:38:49
categories: 
- Git
tags:
- Git
---
# 用git将项目代码上传到Github中
    下列操作前提：已经建立好Github仓库并绑定好SSH-Key并验证了用户名和密码
## 一、第一次提交到新仓库中

### 第一步：建立Git本地仓库
右键项目文件夹 选择 Git Bush Here 执行git命令
```bash
$ git init
```
文件目录里多了.git的隐藏文件
### 第二步：将所有文件添加到仓库中
```bash
$ git add .
```
如果想添加某个特定的文件，只需把.换成特定的文件名即可
<!-- more --> 
### 第三步：将add的文件commit到仓库
```bash
$ git commit -m "注释"
```
-m后必须赋注释的值
### 第四步：将Github仓库和本地仓库关联
以本人Github下Data-Structure仓库为例子
```bash
$ git remote add origin https://github.com/shaoyuanhangyes/Data-Structure.git
```
### 第五步：上传文件到Github仓库
```bash
$ git push -u origin master
```
## 二、将曾提交过(即已存在.git)的新版本提交到仓库中
```bash
$ git remote add origin https://github.com/shaoyuanhangyes/Data-Structure.git
$ git pull --rebase origin master 
#获取远程库与本地同步
$ git push -u origin master
```
## 三、所有操作
```bash
$ git init
$ git add .
$ git commit -m "注释"
$ git remote add origin https://github.com/shaoyuanhangyes/Data-Structure.git
$ git push -u origin master
```