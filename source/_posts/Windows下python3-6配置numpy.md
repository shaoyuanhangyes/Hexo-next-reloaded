---
title: Windows下python3.6配置numpy
date: 2019-01-13 19:39:23
categories:
- Python
tags:
- Python
- Numpy
---
## Windows下使用Pycharm编译器配置Python的Numpy库
### 版本介绍
    Windows系统 Python3.6 Pycharm2017.3.2 numpy-1.14.0
### 环境配置开始
下载numpy
* 1 第一种方法 在cmd里用pip安装
```bash
pip install wheel
pip install numpy
```
如果报错 换第二种方法
<!-- more --> 
* 2 第二种方法 直接下载安装
在 https://www.lfd.uci.edu/~gohlke/pythonlibs/#numpy 下载对应版本号和系统位数的文件
并在下载文件的文件夹使用cmd窗口或者git窗口安装下载的文件 eg：
```bash
pip install 下载的文件名(例如 numpy-1.14.0+mkl-cp36-cp36m-win_amd64.whl)
```
该方法问题在于外网文件下载太慢 但我没出现问题
#### numpy已经安装完毕了 如果打开cmd窗口进入python命令行 敲入测试命令
```bash
>>>import numpy
>>>numpy.random.rand(4,4)
```
会出现4*4的随机数组表示numpy安装成功
#### 但是！在pycharm中 import numpy报错！
经过长时间排查 发现并不是numpy安装的问题 也不是版本错误的问题
而是pycharm没有numpy的模块包
需要设置pycharm
步骤如下

        Pycharm的Setting/project/project Interpreter
        点击绿色加号 Install Available Packages 在列表中找到numpy点击下方的Install Package
操作完成后在pycharm里敲入测试命令
```bash
import numpy
numpy.random.rand(4,4)
```
不会对引入numpy报错 并成功输出随机数组
至此 Windows下Python安装numpy已经完成
### 后记
安装numpy使我初入门python的我大费周章
我刚开始安装的python3.7.0 但是在Pycharm里调用的3.7的exe却出来的是3.6的python
我以为numpy是版本不对应 我重新装了python3.6并重新安装了对应版本的numpy发现在pycharm里还是无法调用
后来才发现pycharm也应该设置 不知道新建一个项目是不是还要我手动添加numpy才好使
感受就是Windows下玩Python真的太累了 还是Linux或者Mac环境下好玩 安装容易指令简单问题也少
如果要是安装小型的库的话直接把文件复制到 python3.6/Lib 目录下就行了
为什么没说是Lib/site-packages 因为我在添加graphics.py的时候发现放在site-packages文件夹下依旧import提示不存在
放在Lib下就不报错了
展望自己的学习路程 应该还需要安装Matplotlib的库 不知道会不会再写一篇关于Matplotlib的文章呢