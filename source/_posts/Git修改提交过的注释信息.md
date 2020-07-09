---
title: Git修改提交过的注释信息
date: 2019-06-06 19:57:52
tags: 
- Git
categories:
- Git
---
## 修改Git时的commit注释
学习git提交文件到github的时候没有注意自己commit的命名，因此事后需要作出修改
### 修改最新一次的commit注释
添加了commit注释，但是没有push 
```bash
$ git commit --amend
```
<!-- more --> 
进入了Vim编辑状态，按i进入插入模式
直接编辑注释信息，按ESC退出插入模式
输入:wq回车后保存退出
### 修改历史提交过的commit注释
```bash
$ git rebase -i HEAD~5
#~5表示修改近5次的提交状态注释
```

命令键入后进入Vim编辑状态，文件内容最上面有着几行pick信息，pick的内容就是曾提交过的commit注释。
按i进入插入模式，将想修改的注释前的pick修改为edit，然后ESC退出插入模式
输入:wq回车后保存退出
```bash
$ git commit --amend
```
这时候就可以修改commit信息
```bash
$ git rebase --continue
```
continue后修改成功