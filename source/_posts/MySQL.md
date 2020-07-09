---
title: MySQL
date: 2020-01-07 21:46:02
categories:
- Database
tags:
- MySQL
mathjax: true
---
## MySQL
### Windows启动MySQL
```bash
#一定要利用管理员权限打开cmd命令行
net start mysql //启动mysql的服务
mysql -u root -p
#输入正确密码后登陆成功
```
### 编码问题
遇到了各种各样的输入中文空白或者乱码 客户端字符集更改为gbk即可
```bash
#在Mysql目录下的my.ini中
[mysql]
# 设置mysql客户端默认字符集
default-character-set=gbk
```
<!-- more --> 
### MySQL语句
#### 创建数据库
```bash
create database db_name character set = utf8; #db_name为数据库名字
```
#### 删除数据库
```bash
drop database db_name;
```
#### 选择数据库
```bash
use db_name;
```
#### 数据类型
##### 数值类型

|类型|字节|范围(无符号)|范围(有符号)|注释|
|:------:|:------:|:------:|:------:|:------:|
|Tinyint|1字节|(-128,127)|(0,255)|小整数|
|Smallint|2字节|(-32 768,32 767)|(0,65 535)|大整数|
|Mediumint|3字节|(-8 388 608,8 388 607)|(0,16 777 215)|大整数|
|int|4字节|(-2 147 483 648,2 147 483 647)|(0,4 294 967 295)|大整数|
|Bigint|8字节|(-9 223 372 036 854 775 808，9 223 372 036 854 775 807)|(0,18 446 744 073 709 551 615)|极大整数|
|Float|4字节|有效数字6-7位||单精度浮点数|
|Double|8字节|有效数字15-16位||双精度浮点数|
##### 日期时间

|类型|字节|范围|格式|注释|
|:------:|:------:|:------:|:------:|:------:|
|Date|3字节|1000-01-01/9999-12-31|YYYY-MM-DD|日期|
|Time|3字节||HH:MM:SS|时间或持续时间|
|Year|1字节|1901/2155|YYYY|年份|
|Datatime|8字节|1000-01-01 00:00:00/9999-12-31 23:59:59|YYYY-MM-DD HH:MM:SS|具体时间|
|TimeStamp|4字节|1970-01-01 00:00:00/2038-01-19 03:14:07|YYYY-MM-DD HH:MM:SS|时间戳|
插入时间数据必须加双引号
##### 字符串类型

|类型|字节|注释|
|:------:|:------:|:------:|
|Char|0-255字节|定长字符串|
|Varchar|0-65535字节|变长字符串|
|Blob|0-65535字节|二进制形式的长文本数据|
|Text|0-65535字节|长文本数据|
```bash
#Mysql5.0以上 汉字字节数与编码有关 UTF-8中一个汉字3个字节 GBK中一个汉字2个字节
```
#### 创建/删除模式
```bash
create schema <模式名> authorization <用户名>
drop schema <模式名> <cascade/restrict>
#cascade级联 删除模式的同时把该模式下所有的数据库对象全部删除
#restrict限制 如果该模式下存在已定义的数据库对象(表/视图) 就拒绝删除语句的执行
```
#### 创建表

```bash
create table Student(#学生信息表
    Sno varchar(9) primary key,#Sno为主键
    Sname varchar(20) unique,#唯一
    Ssex varchar(2),
    Sage smallint,
    Sdept varchar(20)
)charset utf8;
```

|学号Sno|姓名Sname|性别Ssex|年龄Sage|专业Sdept|
|:------:|:------:|:------:|:------:|:------:|
||||||

```bash
#外键示范
create table Course(
    Cno varchar(4) primary key,#Cno为主键
    Cname varchar(40) not null,
    Cpno varchar(4),#Cpno为外键 参照表是Course下的Cno
    Ccredit smallint,
    foreign key(Cpno) references Course(Cno)#参照表可以是自己表里的字段
)charset utf8;
```
#### 修改表
```bash
alter table Student
add entrance date not null;#为Student表添加一列entrance 类型为date 约束为not null
#增加新的一列的数据都为空
```
```bash
alter table Student
drop ent;#删除Student表中名为ent的一列
```
#### 删除表
```bash
drop table Student;
```
#### 索引
表的数据量较大时 查询操作比较耗时 建立索引加快查询速度 能快速定位到需要查询的内容
##### 建立索引
```bash
create unique index Stusno on Student(Sno) asc/desc);#Stusno为索引名 asc升序desc降序
```
##### 修改索引
```bash
alter index Stusno rename to Ssno;#更改旧索引名Stusno为Ssno
```
##### 删除索引
```bash
drop index Ssno;#删除名为Ssno的索引
```
#### 插入数据
```bash
insert into Student (字段名,字段名) values(值,值);
```
#### 查询
查询全体记录
```bash
select * from <表名>
```
查询部分字段记录
```bash
select <字段名>,<字段名> from <表名>
```
查询表内某一字段存在大量重复元素时 使用distinct
```bash
select distinct <字段名> from <表名>
```
##### where

|学号Sno|姓名Sname|性别Ssex|年龄Sage|专业Sdept|
|:------:|:------:|:------:|:------:|:------:|
|201215121|张三|男|20|CS|
|201215122|李四|女|19|IS|
|201215123|王五|女|18|MA|
|201215124|赵六|男|22|IE|

```bash
select Sno,Sname from student where Sage<20;
select Sno,Sname from student where Sdept="CS";
select Sno,Cno from SC where Grade is null;#is不能换=

select Sno,Sname,Sage from student where Sage between 19 and 20;#Sage在19-20之间(包括19和20)
select Sno,Sname from student where Sage not between 19 and 20;#Sage不在19-20之间
select Sno,Sname from student where Sdept in ("CS","IS");
select Sno,Sname from student where Sdept not in ("CS","IS");
```
##### 模糊查询
字符匹配 使用like和%
a%  以a为开头的字符串
a%b 以a为开头b为结尾的字符串 长度不限
a_b 以a为开头b为结尾的长度为3的字符串  `#字符串为汉字时要注意编码格式 注意一个汉字占几位`
```bash
select Sno,Sname from student where Sname like "刘%";#查询Sname姓刘的所有人
select Sno,Sname from student where Sname like "刘_";#查询Sname姓刘的两个字名字的人
select Sno,Sname from student where Sname like "_阳%";#查询第二个字为阳的名字
select Sno,Sname from student where Sname not like "刘%";
```
##### Order By
```bash
#数据库系统概论第5版王珊编著中提到Order By语句默认从小到大升序asc排列
select * from sc order by Grade desc;#查询结果按成绩从大到小 降序排列
select * from sc order by Grade asc;#查询结果按成绩从小到大 升序排列
select * from student order by Sdept,Sage desc; #同一个Sdept中Sage从大到小排列
#空值被认为是最大的
```
##### 聚焦函数
函数不能用于Where语句 只能用于Select语句 或者 Group By中的Having语句
```bash
Count()#统计个数 空值也计算进去
Sum()#计算一列数的总和  要求存储的数据必须是数值型数据 不能是字符型数据
Avg()#计算一列数的平均值  要求存储的数据必须是数值型数据 不能是字符型数据
Max()#一列数值的最大值
Min()#一列数值的最小值
```
Distinct 查询中筛除掉指定列的重复值
```bash
select Count(*) from student;#查询学生总人数
select Count(Distinct Sno) from SC;#查询了选修的人数
select Avg(Grade) from SC where Sno=201215121;#查询学号201215121的平均成绩
select Max(Grade) from SC where Cno=2;#查询课程号为2的最高成绩
select Sum(Ccredit) from SC,Course
where Sno=201215121 and SC.Cno=Course.Cno;#查询Sno为201215121选课的总学分数
```
##### Group By
分组后使用Having指定筛选条件
```bash
select Cno,Count(Sno) from SC Group By Cno;#查询课程号和选课的人数 按照Cno的值分组 有相同Cno的值的在一个组 然后用Count函数对每个组的总人数进行计算
select Sno from SC Group By Sno Having Count(*)>=3;#Group By按照Sno分组 Having筛选 元组个数>=3 才会被查询到
```
Where Having 区别 Where语句作用于基本表与视图 从中选择满足条件的组 Having作用于组 从中满足条件的组
##### 连接查询
一次查询涉及到多个数据表
