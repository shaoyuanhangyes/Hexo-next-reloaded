---
title: Go基本语法
date: 2020-07-19 11:10:19
categories:
- Go
tags:
- Go
mathjax: true
---
# Go基本语法

## 声明变量

```go
//直接声明
var name string = "Ruojhen"
var id int = 123
var name = "shaoyuanhang@outlook.com" //不指定类型声明 靠编译器自动识别
/*但编译器自动识别会产生我们不需要的效果 例如: */
var decimal = 0.06 //因为右值带有小数点 所以在不指定类型的情况下 编译器将这个变量声明为float64 但很多时候我们不需要这么高的精度 因为高精度意味着占用内存空间就更大
var decimal float32 = 0.03
/*以上声明方式可以不赋予变量值 这样变量就会被初始化为初值 string就是空字符串 int就是0 float32/float64就是0.0 bool就是false 指针就是nil*/
```
<!-- more --> 
```go
//多个变量一起声明 只能用于函数内部
var(
		id int =132456
		name = "syh"
		gender = "男"
	)
    fmt.Print(id,name,gender)
    
//打印结果为 132456syh男
```

```go
//快速初始化变量
name:="syh" /*效果等价于*/  var name = "syh" var name string = "syh"

id,name:= 123,"syh" //相当于将123赋值给id "syh"赋值给name
/*这个方法还能用于交换两个数*/
var a int =100
var b int =200
b,a=a,b
fmt.Print(a," ",b)//输出结果为 200 100
```

```go
//声明指针变量
var age int =100
var address  =&age
fmt.Print(age," ",address)//输出结果为 100 0xc00000a0c8

//使用new函数声明匿名变量
ptr:=new(int)
fmt.Println("ptr address",ptr) //ptr address 0xc0000a0090
fmt.Println("ptr value",*ptr)  //ptr value 0

/*所谓匿名变量 又称作占位符 空白标识符 在代码中我们使用下划线代替他 
表示一些我们在函数返回值中必须要接收 但以后却用不到的值  匿名变量优点
: Ⅰ不分配内存空间 Ⅱ不需要为命名无用的变量名纠结 Ⅲ多次声明不会有任何问题
```
## 数据类型
### 整型

### 浮点型

### byte/rune

### 字符串

### 数组

### 切片

### 指针