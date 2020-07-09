---
title: C++ 面试题集合
date: 2020-05-22 14:40:56
categories:
- C++
tags:
- C++
mathjax: true
---
## std::function可以封装哪些实体 可以封装函数对象么
std::function介绍
类模版std::function是一种通用、多态的函数封装。std::function的实例可以对任何可以调用的目标实体进行存储、复制、和调用操作，这些目标实体包括普通函数、Lambda表达式、函数指针、以及其它函数对象等。std::function对象是对C++中现有的可调用实体的一种类型安全的包裹（我们知道像函数指针这类可调用实体，是类型不安全的）
通过std::function对C++中各种可调用实体（普通函数、Lambda表达式、函数指针、以及其它函数对象等）的封装，形成一个新的可调用的std::function对象；让我们不再纠结那么多的可调用实体。一切变的简单粗暴

## 为什么需要this指针
this指针的作用是指向成员函数所作用的对象 所以非静态成员函数中可以直接使用this来代表指向该函数作用的对象的指针

## static和全局变量有什么区别
static变量和全局变量都是静态存储方式 static修饰的全局变量只在这个文件中有效 

## 程序
```bash
#include <iostream>
using namespace std;
int main(){
    for(int i=0;i<10;i++){
        static int a=0;
        a++;
    }
    cout<<a<<endl;
}
```
无法输出a 因为a是局部的静态变量
## 左值引用 右值引用
```bash
int a=10;
int &b=a;//左值引用
int &var=10; 错误 无法对一个立即数取地址，因为立即数并没有在内存中存储，而是存储在寄存器中
const int &var=10; 可以
int &&c=10;//右值引用 
int &&c=a;  不可用 表达式错误 表达式a是左值
```
左值引用要求右边的值必须能够取地址，如果无法取地址，可以用常引用；
但使用常引用后，我们只能通过引用来读取数据，无法去修改数据，因为其被const修饰成常量引用了。
## static_cast与强制类型转换的区别
static_cast在编译时会进行类型检查，而强制转换不会。
并且 消除const属性用const_cast  基本类型转换用static_cast  多态类之间的类型转换用dynamic_cast 不同类型的指针类型转换有那个reinterpreter_cast
## 用户态切换到内核态的方式
1.系统调用
2.异常
3.外围设备终端