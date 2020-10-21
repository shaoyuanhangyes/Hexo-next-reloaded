---
title: Fibnacci Sequence
date: 2020-07-22 12:28:57
categories:
- Algorithm
tags:
- Algorithm
- C++
mathjax: true
---
## 斐波那契数列

学习动态规划的时候见到了一个词语 叫做记忆化搜索 用于递归的`剪枝` 用空间的增大来换取时间的压缩 

下面是关于斐波那契数列的代码例子 
<!-- more --> 
### 传统递归

```C++
#include <iostream>

using namespace std;

int f(int n){
   if(n==0) return 0;
   if(n==1) return 1;
   return f(n-1)+f(n-2);
}

int main(){
   int n;
   cin>>n;
   cout<<f(n);
   return 0;
}
```
传统递归的弊端就是会重复计算 例如我要计算f(5)=f(4)+f(3) 计算f(4)的时候又要在计算一遍f(3) 因此采用记忆化搜索来防止这些重复计算来提高速度

### 记忆化递归

```C++
#include <iostream>
#include <vector>
using namespace std;
const int MAX=100;
vector<int> dp(MAX,-1);
int f(int n){
    if(dp[n]!=-1) return dp[n];//如果数组表内元素已经更新了 我们就不需要再次计算直接拿来用就可以
    else{
    	dp[n]=f(n-1)+f(n-2);
    	return dp[n];
	}
}
int main(){
    int n;
    dp[0]=0;dp[1]=1;
    cin>>n;
    cout<<f(n)<<endl;
    for(int i=0;i<=n;++i) cout<<"dp["<<i<<"]="<<dp[i]<<endl;//我使用Clion调试的不能显示全局变量的值 只能直接print出来观察
    return 0;
}
```
经过测试 例如输入f(44)计算的时候 记忆化搜索的速度明显比传统递归的速度要快的多
### 其他方法
我在查阅资料的时候发现很多人说还能再提升速度 使用矩阵乘法的方式 有时间我会更新此篇文章 加上速度更快的方法