---
title: Mongodb
date: 2020-02-07 16:48:23
categories: 
- Mongodb
tags:
- Linux
- Mongodb
---
# Ubuntu下Mongodb
## 安装
<!-- more --> 
## 使用
进入Mongodb命令行界面
```bash
mongo
```
创建数据库或进入现有数据库
```bash
use db_name #db_name为数据库名字
```
查看当前连接的数据库
```bash
db
```
查看所有的数据库
```bash 
show dbs
```
销毁数据库
```bash
use local
db.dropDatabase() #销毁名为local的数据库
```
创建集合
```bash
use db_name
db.createCollection("users") #在名为db_name的数据库下创建users集合
show collecions #查看集合
db.users.drop() #删除users集合
```
向集合中插入数据
insert()方法
```bash
use db_name
db.users.insert([
    {name: "syh",
    email: "shaoyuanhangyes@outlook.com"
    },
    {name: "Ruojhen",
    gender: "male"
    }
])
```
save()方法
```bash
use db_name
db.users.save([
    {name: "syh",
    email: "shaoyuanhangyes@outlook.com"
    },
    {name: "Ruojhen",
    gender: "male"
    }
])
```
insert()与save()区别
insert侧重于新增记录 save可以保存一个新增记录也可以保存记录的修改 类似于Mysql的Update
查询语句
find()
```bash
use db_name
db.users.find() #直接返回users集合中所有文档
db.users.find().pretty() #使返回的文档数据更加美观易读
```
使得find()始终以pretty()返回
```bash
echo "DBQuery.prototype._prettyShell = true" >> ~/.mongorc.js
```