---
title: STL
date: 2020-04-22 12:31:23
categories:
- C++
tags:
- C++
- STL
mathjax: true
---
## C++ STL
STL是Standard Template Library 是一种泛型编程
STL从广义上分为 容器(container) 算法(algorithm) 迭代器(iterator)
目前广泛认为STL有六大组件
### 容器
可以说容器就是将一些常用的数据结构用模版实现出来
容器中包括的常用数据结构有: `字符串(string)` `定长数组(array)` `动态数组(vector)` `链表(list)` `栈(stack)` `队列(queue)` `双端队列(deque)` `树(tree)` `集合(set)` `映射(map)`
根据数据在容器中的排列特性 这些数据分为序列式容器和关联式容器两种
序列式容器强调值的排序 序列式容器中的每个元素均有固定的位置 除非用删除或插入的操作改变这个位置 Vector容器 Deque容器 List容器等
关联式容器是非线性的树结构 更准确的说是二叉树结构 各元素之间没有严格的物理上的顺序关系 也就是说元素在容器中并没有保存元素置入容器时的逻辑顺序 关联式容器另一个显著特点是：在值中选择一个值作为关键字key 这个关键字对值起到索引的作用 方便查找 Set/multiset容器 Map/multimap容器
<!-- more --> 
#### string
##### string构造函数
```C++
//--------------class string-----------//
string();//无参数构造函数
string(const string &str);//使用对象str复制构造函数
string(const char *s);//一个参数的复制构造函数 使用字符串s来初始化
string(int n,char c);//多参数构造函数 使用n个字符c初始化
//--------------main()-----------------//
string s1;//使用无参数构造函数初始化
string s2("syh");//使用一个参数的构造函数
string s3="syh02";//使用一个参数的复制构造函数
string s4(10,'k');//是用哪个多参数构造函数 初始化为10个k
string s5=s2;//用s2初始化s5 使用复制构造函数

```
##### string重载运算符的基本赋值操作
```C++
string &operator=(const char *s);//string s1;s1="syh";
string &operator=(const string &s);//string s1,s2;s1=s2;
string &operator=(char c);

//---------------assign()方法-----------------//
string& assign(const char *s);//把字符串s赋给当前的字符串
string& assign(const char *s, int n);//把字符串s的前n个字符赋给当前的字符串
string& assign(const string &s);//把字符串s赋给当前字符串
string& assign(int n, char c);//用n个字符c赋给当前字符串
string& assign(const string &s, int start, int n);//将s从start开始n个字符赋值给字符串
//调用方式 string s1,s2;s1.assign(s2) s1.assign(s2,1,2);#将s2从索引为1的位置(也就是第二位开始)2个字符赋值给s1
```
##### string获取相应位数的字符操作
```C++
char &operator[](int n);
char &at(int n);
//eg: string s1="abcdefg";cout<<s1.operator[](3); #结果为d
//eg: string s1="abcdefg";cout<<s1.at(3);
```
##### string字符串的拼接(在结尾处添加)
```C++
string &operator+=(const string &str);//重载+=运算符 string s1,s2;s1+=s2;
string &operator+=(const char *str);//string s1;s1+="syh";
string &operator+=(const char c);
string &append(const char *s);//string s1;s1.append("syh");#把"syh"拼接在s1后面
string &append(const char *s,int n);string s1;s1.append("syh",2);#把"syh"的前2位拼接在s1后面
string& append(const string &s);//string s1,s2;s1.append(s2);#把s2拼接在s1后面
string& append(int n, char c);//在当前字符串结尾添加n位的字符c
string& append(const string &s, int pos, int n);//string s1,s2;s1.append(s2,1,2);#将s2从索引为1的位置开始2个字符拼接在s1的后面
```
##### string查找和替换
```C++
//-------------------查找----------------//
//查找成功返回索引序号 查找失败返回string::npos=18446744073709551615 即2^(64)-1
int find(const string& str, int pos = 0) const; //查找str第一次出现位置,从pos开始查找
int find(const char* s, int pos = 0) const;  //查找s第一次出现位置,从pos开始查找
int find(const char* s, int pos, int n) const;  //从pos位置查找s的前n个字符第一次位置
int find(const char c, int pos = 0) const;  //查找字符c第一次出现位置
//-----------------反向查找 查找最后出现的位置------------------//
int rfind(const string& str, int pos = npos) const;
int rfind(const char* s, int pos = npos) const;//查找s最后一次出现位置,从pos开始查找
int rfind(const char* s, int pos, int n) const;//从pos查找s的前n个字符最后一次位置
int rfind(const char c, int pos = 0) const;

//rfind()的返回值我和给定的pos值我还未完全掌握
eg:s1="abcdefgabcdfe"; int i=s1.rfind("a",12); #i=7
//-------------------替换------------------//
string &replace(int pos,int n,const string &str);
string &replace(int pos,int n,const string *s);
//eg:string s1="abcdefg";string s2="syh";s1=s1.replace(1,2,s2);#将s1的从索引为1位置起的2个字符替换为s2 结果为s1=asyhdefg
```
##### string比较大小
```C++
//大写字母比小写字母小 根据ascii码字典排名越靠后越大 大于时返回1 小于时返回-1 相等时返回0
int compare(const string &str);
int compare(const char *s);
//eg: string s1="abc";string s2="syh";#s1.compare(s2)=-1 因为abc比syh小
```
##### string子串
```C++
string substr(int pos=0,int n=npos) const;//截取从pos位置开始n位
//eg:string s1="abcdefg";s1.substr(1,2)="bc"
```
##### string插入和删除
```C++
string &insert(int pos,const char *s);
string &insert(int pos,const string &str);
string &insert(int pos,int n,char c);
//在pos位置出插入字符串
//eg: string s1="abcdefg";s1.insert(2,"syh")="absyhcdefg"
string &erase(int pos,int n=npos);//删除从pos位置起的n个字符
//eg: string s1="abcdefg";s1.erase(2,3)="abfg"
```
##### c++ string和C-type字符串互相转化
```C++
//string转char *
string str="it";
const char *cstr=str.c_str();
//char *转string
char *s="it";
string str(s);
//c++中存在一个从`const char *`到`string`的隐式转换 不存在string到c风格的自动类型转换

```


#### vector
vector起到了动态数组的作用 加入新元素内部会自动扩容 vector支持随机存取
所以vector提供的是随机访问迭代器(Random Access Iterators)

##### vector常用方法
```C++
#include<vector>
vector<int> v;
v.push_back(i);//在v后面增加数据i
v.capacity();//返回vector的容量 我们会发现vector数组的长度和容量不是一样的 往往容量比长度要大一点
```
因为vector采取的是线性连续空间 它以两个迭代器_Myfirst和_Mylast分别指向配置得来的连续空间中目前已被使用的范围 并以迭代器_Myend指向整块连续内存空间的尾端 意思就是_Myfirst指向数组的头部 _Mylast指向数组的尾部 _Myend指向数组大小空间真正的最后一块
##### vector构造函数
```C++
vector<T> v;//采用模版类实现 默认构造函数
vector v1(n,elem);//构造函数将n个elem赋值给本身=v1={elem....}
vector<T> v1(v.begin(),v.end());
vector<T> v1(v);//拷贝构造函数 vector(const vector &vec);
vector<T> v={1,2,3};
```
##### vector赋值
```C++
assign(begin,end);//vector<T> v1,v2;v2.assign(v1.begin(),v1.end())
assign(n,elem);//vector<T> v1;v1.assign(4,5);#将4个5赋值给v1
vector &operator=(const vector &vec);//vec重载运算符= vector<T> v1,v2;v2=v1;

swap(&vec);//交换两个vector数组的内容 
 vector<int> v1={1,3,5,7,9};vector<int> v2={2,4,6,8,10};
 v1.swap(v2); v1与v2数组的内容互换了 v1是246810 v2是13579

```
##### vector长度空间操作
```C++
size();//返回vector数组内元素的个数 v.size();
empty(); //判断数组是否为空 空返回1 非空返回0
resize(int n);//重新指定数组长度 如果比原来的长 填充默认值进去 如果比原来的短 就把超出的数据删除
resize(int n,elem);//重新指定数组长度 如果比原来的长 就填充那些数量的elem
v.capacity();//返回vector的容量 我们会发现vector数组的长度和容量不是一样的 往往容量比长度要大一点
reserve(int len);//预留len个元素长度 预留的位置不可初始化 也不能被访问

```
vector容器如果空间不足 将会重新申请一块更大的空间然后将元素都复制过去 再释放旧有的空间 
##### 用swap缩短内存空间
```C++
#include<iostream>
#include<vector>
vector<int> v;
    for (int i=0;i<100000;i++){
        v.push_back(i);
    }
    cout << "capacity:" << v.capacity() << endl; //capacity:131072
    cout << "size:" << v.size() << endl; //size:100000
    v.resize(100);//改变容量

    cout << "capacity:" << v.capacity() << endl; //capacity:131072
    cout << "size:" << v.size() << endl; //size:100

    vector<int> (v).swap(v);
    cout << "capacity:" << v.capacity() << endl; //capacity:100
    cout << "size:" << v.size() << endl; //size:100
```
##### vector取数据
```C++
at(int n);//返回索引为n的数据 如果越界 抛出异常 out_of_range
operator[](int n);//返回索引为n的数据 如果越界 抛出异常 out_of_range
front();//返回vector数组第一个数据
back();//返回vector数组最后一个数据
```
##### vector插入删除
```C++
push_back(ele);//在数组尾部插入ele
pop_back();//删除最后一位数据
insert(const_iterator pos, int count,ele);//在pos迭代器指向的位置插入count个ele
erase(const_iterator start, const_iterator end);//删除迭代器start到end范围内的数据
erase(const_iterator pos);//删除迭代器pos指向的数据
clear();//删除vector数组中全部的数据
```
vector在尾部插入数据 也可以在头部插入数据 但是效率很低很低 因此引入deque容器概念
#### deque
deque可以高效率的对头端进行元素的插入和删除 并且没有容量的概念 因为deque逻辑上是由动态的以分段连续空间组合而成 随时可以增加一段新空间连接起来
实际上deque是由一段一段的定量的连续空间构成 一旦有必要在deque前端或者尾端增加新的空间 便配置一段连续定量的空间 串接在deque的头端或者尾端
Deque最大的工作就是维护这些分段连续的内存空间的整体性的假象 并提供随机存取的接口 避开了重新配置空间 复制 释放的轮回 代价就是复杂的迭代器架构
deque的迭代器非常复杂 所以除非有必要 应该尽可能的先使用vector 对deque的排序操作 可以通过先将deque的元素复制到vector中 排序后再复制回deque中来提高效率

deque采取了一块连续的内存空间作为主控 其中每一个元素都是一个指针 指向另一段连续的存储空间(缓冲区) 这些存储空间存储了deque的元素

##### deque构造函数
```C++
deque<T> D;//默认构造函数
deque<T> D(D1.begin(),D1.end());//构造函数将begin-end的范围内的元素拷贝给自身
deque<T> D(n,elem);//构造函数将n个elem初始化给D 
deque(const deque &deq);//拷贝构造函数
```
##### deque赋值
```C++
assign(begin(),end());//deque<T> d1,d2;d1.assign(d2.begin(),d2.end());
assign(n,elem);//将n个elem赋值给D deque<T> D; D.assign(3,5);
deque &operator=(const deque &deq);//重载运算符 d1=d2;
swap(deq);// d1.swap(d2); d1与d2互换
```

##### deque长度空间操作
```C++
size();//返回deque中元素个数
empty();//判断deque是否为空 空返回1 非空返回0
resize(int n);//重新指定数组长度 如果比原来的长 填充默认值进去 如果比原来的短 就把超出的数据删除
resize(int n,elem);//重新指定数组长度 如果比原来的长 就填充那些数量的elem
```
##### deque双端插入删除
```C++
push_back(elem);//在deque尾部添加一个elem
push_front(elem);//在deque首部添加一个elem

pop_back();//删除deque最后一个元素
pop_front();//删除deque第一个元素

insert(const_iterator pos, int count,ele);//在pos迭代器指向的位置插入count个ele
erase(const_iterator start, const_iterator end);//删除迭代器start到end范围内的数据
erase(const_iterator pos);//删除迭代器pos指向的数据 返回下一个数据的位置
clear();//删除deque中全部的数据
```
##### deque取数据
```C++
at(int n);//返回索引为n的数据 如果越界 抛出异常 out_of_range
operator[](int n);//返回索引为n的数据 如果越界 抛出异常 out_of_range
front();//返回deque第一个数据
back();//返回deque数组最后一个数据
```
#### stack
stack栈是一个先进后出的数据结构 stack容器允许新增/移除元素 取栈顶元素 stack容器不允许遍历元素 stack没有迭代器
##### stack构造函数
```C++
stack<T> stk;//默认构造函数
stack(const stack &stk);//拷贝构造函数
```
##### stack赋值
```C++
stack& operator=(const stack &stk);//运算符=重载
```
##### stack存取
```C++
push(elem);//栈顶添加elem
pop();//出栈
top();//返回栈顶元素
```
##### stack空间
```C++
empty();//判断deque是否为空 空返回1 非空返回0
size();//返回栈元素的个数
```
#### queue
队列是一种先进先出的数据结构 队头出 队头进
##### queue构造函数
```C++
#include<queue>
queue<T> q;//默认构造函数
queue(const queue &q);//拷贝构造函数
```
##### queue赋值
```C++
queue &operator=(const queue &q);//重载运算符=
```
##### queue存取
```C++
push(elem);//队尾增加elem
pop();//队头移除一个元素
back();//返回最后一个元素
front();//返回第一个元素
```
##### queue空间
```C++
empty();//判断队列是否为空
size();//返回队列大小
```
#### list
list容器是一个循环的双向链表
list对元素的插入和删除效率都非常高
list容器不能像vector一样用普通的指针作为迭代器 因为它的节点不能保证在同一块连续的内存空间上 list容器提供的迭代器是Bidirectional Iterators
##### list构造函数
```C++
list<T> l;//默认构造函数
list(l.begin(),l.end());//构造函数将begin到end区间内的元素拷贝给本身
list(n,elem)//构造函数将n个elem拷贝给本身
list(const list &l);//拷贝构造函数
```
##### list赋值
```C++
assign(begin,end);//list<T> l1,l2;l2.assign(l1.begin(),l2.begin())
assign(n,elem);
list &operator=(const list &l);//重载运算符=
swap(l);//交换两个链表的内容 l1.swap(l2)
```
##### list空间
```C++
size();//返回链表内元素个数
empty()//判断数组是否为空 空返回1 非空返回0
resize(int n);//重新指定链表长度 如果比原来的长 填充默认值进去 如果比原来的短 就把超出的数据删除
resize(int n,elem);//重新指定链表长度 如果比原来的长 就填充那些数量的elem
```
##### list存取数据
```C++
front();//返回第一个元素
back();//返回最后一个元素
```
##### list插入删除
```C++
push_back(elem);//在链表尾部插入一个元素elem
pop_back();//删除链表最后一个元素
push_front(elem);//在链表头部插入一个元素elem
pop_front();//删除链表的第一个元素

insert(const_iterator pos, int count,ele);//在pos迭代器指向的位置插入count个ele
insert(pos,begin(),end());//在pos位置插入begin到end区间的元素
insert(pos,elem);//在pos位置插入elem 返回新数据的位置

erase(begin,end);//删除begin到end区间的元素 返回下一个数据的位置
erase(pos);//删除pos位置的元素 返回下一个数据的位置
clean();//清除所有数据
remove(elem);//删除list中与elem匹配的元素
```
##### list排序
```C++
reverse();//翻转链表
sort();
```
#### set/multiset
set是一个关联容器 不需要做内存拷贝和内存移动 set用来存储同一数据类型的容器
在set容器中 每个元素的值都是唯一的 并且系统根据元素的值进行自动排序
multiset特性与用法和set完全相同 唯一的区别是multiset允许元素的值重复
C++ STL中标准关联容器`set` `multiset` `map` `multimap`内部采用的就是一种非常高效的平衡检索二叉树：红黑树 也称为RB树(Red-Black Tree) RB树的统计性能要好于一般平衡二叉树
set中元素的值不能被直接改变 因为set元素值就是它的key值 关系到set元素的排序规则
简言之set容器的迭代器是const_iterator
##### set构造函数
```C++
set<T> st;//默认构造函数
set(const set &st);//拷贝构造函数
```
##### set赋值
```C++
set &operator=(const set &s);//运算符重载
swap(s);//交换两个集合容器的值
```
##### set查找操作
```C++
find(key);//查找key值是否存在 若存在就返回该键元素的迭代器 若不存在 返回set.end()
count(key);//查找键key的元素个数 在set容器中就变成了查询某个key值是否存在过了
lower_bound(keyElem);//返回第一个值大于keyElem的迭代器
upper_bound(keyElem);//返回最后一个值大于keyElem的迭代器
equal_range(keyElem);//返回值是一个pair类型
```
##### set插入删除
```C++
//迭代器声明 set<int>::iterator first;first是你声明的迭代器名字
insert(elem);//插入elem
clear()
erase(pos);//删除迭代器pos指向的元素 返回下一个元素的迭代器
erase(begin,end);//删除begin到end区间的所有元素 返回下一个元素的迭代器
erase(elem);//删除容器中值为elem的元素
```
##### 用迭代器遍历输出set内数据
```C++
#include<set>
//---------main()------------//
    set<int> s;
    s.insert(5);
    s.insert(2);
    s.insert(3);
    set<int>::iterator x;
    for(x=s.begin();x!=s.end();++x){
        cout<<*x<<" ";
    }
```
#### pair
pair容器的类型定义需要引入头文件`#include<utility>`
pair存有两个值 这一对值可以有不同的数据类型 两个值分别可以用first和second访问
##### pair构造函数
```C++
pair<T1,T2> p1;//T1 T2是两个类型 默认构造函数
pair<T1,T2> p1(v1,v2);//创建一个pair对象 first成员初始化为v1 second成员初始化v2
pair<T1,T2> p1= make_pair(v1,v2);//以v1 v2的值创建一个新的对象p1
pair<T1,T2> p1=p2;//拷贝构造函数
```
#### map/multimap
map容器中存放的元素都是pair类型的 这一对值中第一个被视为键(key) 第二个被视为值(value) map不允许两个元素有相同的key map有助于处理一对一映射数据 
map容器中所有元素会根据元素的key自动排序 不能通过map的迭代器改变map的值 因为map的key关系到map元素的排列规则 若想要修改map的value是可以的
与set容器相同 multimap的键值可以重复
##### map构造函数
```C++
map<T1,T2> mapt;//map默认构造函数
map(const map &mapt);//拷贝构造函数
map(iterator begin,iterator end);//区间构造函数
```
##### map赋值
```C++
map &operator=(const map &mapt);//重载运算符=
swap(mp);//交换
```
##### map空间操作
```C++
size();
empty();
max_size();
```
##### map插入删除
```C++
//-----------------四种插入方式演示----------------//
#include<map>
    map<int,string> student;//初始化
    //第一种初始化 使用value_type
    student.insert(map<int,string>::value_type(100,"syh"));
    //第二种初始化 使用pair
    student.insert(pair<int,string>(200,"sc"));
    //第三种初始化 使用make_pair
    student.insert(make_pair(300,"abc"));
    //第四种初始化 使用数组初始化 数组的序号为first 数组的赋值为second
    student[3]="test";
    //使用迭代器遍历输出
    map<int,string>::iterator iter;
    for(iter=student.begin();iter!=student.end();++iter){
        cout<<iter->first<<" "<<iter->second<<endl;
    }
```
程序输出结果为:
    3 test  
    100 syh
    200 sc
    300 abc
##### map查找
```C++
find(key);//查找key值是否存在 存在返回该元素的迭代器 不存在返回 end()
count(key);//查找map中key的对数 对map来说要么是0要么是1 对multimap来说 可能大于1
//count对map来说也可以用if来判断是否能查找到想要的key

lower_bound(keyElem);//返回第一个值大于keyElem的迭代器
upper_bound(keyElem);//返回最后一个值大于keyElem的迭代器
equal_range(keyElem);//返回值是一个pair类型
```
##### map删除数据
```C++
erase(map<T1,T2>::iterator iter);// 删除迭代器所指的节点
erase(key);//删除key值对应的节点
erase(map<T1,T2>::iterator iter1,map<T1,T2>::iterator iter2);//删除iter1到iter2之间的数据
```

#### STL容器表格对比

||vector|deque|list|set|multiset|map|multimap|
|:------:|:------:|:------:|:------:|:------:|:------:|:------:|:------:|:------:|:------:|
|数据结构|单端数组|双向队列|循环双向链表|二叉树|二叉树|二叉树|二叉树|
|随机存取|是|是|否|否|否|key(否)|key(否)|
|元素寻找|慢|慢|非常慢|快|快|key(快)|key(快)|
|元素增减|尾端|头/尾|任何位置|-|-|-|-|


### 算法
STL收录了大量常用的算法例如排序sort() 查找find()等等 特定的算法搭配特定的数据结构
算法分为质变算法和非质变算法
质变算法是指运算过程中会改变元素的内容 例如拷贝替换删除等
非质变算法是指运算过程中不会改变元素的内容 例如查找遍历寻找极值等等

### 迭代器
迭代器(iterator)是一种抽象的设计概念 《设计模式》一书中囊括了共23种设计模式的完整描述
其中关于迭代器的定义为:提供一种方法 使之能够依照次序寻访某个容器中的各个元素 并且还可以不暴露容器的内部表示方式
综上 迭代器就是在某一容器中进行某种算法的操作 可谓是容器和算法的粘合剂 容器和算法分开独立设计 再由迭代器粘合在一起 这也是STL的中心思想
迭代器的种类分为:
输入迭代器 输出迭代器 前向迭代器 双向迭代器 随机访问迭代器

