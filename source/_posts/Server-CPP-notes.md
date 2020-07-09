---
title: Server_CPP notes
date: 2020-05-13 17:30:06
categories:
- C++
tags:
- C++
- Linux
mathjax: true
---
项目地址 `https://github.com/shaoyuanhangyes/Server`
## 项目介绍

<!-- more --> 
## 进程
#include<unistd.h>
pid_t pid 声明进程标识符pid
### 进程复制函数 fork()
fork函数只能被调用一次 却能返回两次 有三种不同的返回值
在父进程中fork返回新创建的子进程的id
在子进程中fork返回0 
出现错误 返回-1 并设置errno
fork函数复制当前进程 在内核进程表中创建一个新的进程表项 新的进程表项有很多属性与原进程相同 比如堆栈的指针 标志寄存器的值 但也有属性变化
比如进程的ppid被设置为父进程的pid 信号位图被清楚 原进程设置的信号处理函数不再对子进程起作用 
子进程代码与父进程代码完全相同 还复制了父进程的数据 但数据的复制只有在任意进程无论父子进程对数据进行了写操作的时候才会发送复制

### exec系列函数 替换系统映像
```bash
int execl(const char *path,const char *arg,...)/*path参数指定可执行文件的完整路径*/
int execlp(const char *file,const char *arg,...)/*file参数指定可以接受文件名*/
int execle(const char *path,const char *arg,...,char * const envp[])/*arg接受可变参数 argv接受参数数组*/
int execv(const char *file,char *const argv[])
int execve(const char *path,char *const argv[],char * const envp[])
```
这些参数都会被传递给新程序的main函数中 envp参数用于设置新程序的环境变量 
exec函数一般不返回值 除非出错误时返回-1并设置errno
### 僵尸进程
对于多进程程序来说 父进程一般需要跟踪子进程的退出状态 因此 当子进程结束运行时 内核不会立即释放该进程的进程表项 这样来满足
父进程后续对该子进程退出信息的查询 在子进程结束运行后并且其父进程读取其退出状态前我们称之为子进程处于僵尸态 
另外一种子进程进入僵尸态的情况是 父进程结束或者异常终止 但子进程却继续运行 此时子进程的ppid被系统设置为1 即init进程
init进程接管了该子进程 并等待他结束 在父进程退出后子进程退出前该子进程是处于僵尸态
所以僵尸态就是父进程没有正确处理子进程返回的信息并且子进程依旧占据着内核资源
### 管道pipe
管道是进程内部通信的方式 管道也是父进程和子进程间通信的常用手段 利用的是fork调用后两个管道文件描述符(fd[0] fd[1])都保持打开
一堆这样的文件描述符只能保证父子进程间一个方向的数据传输 父进程和子进程必须有一个关闭fd[0] 另一个关闭fd[1]
显然 要实现父子进程间的双向数据传输就必须使用两个管道

### 信号量
#include<sys/sem.h>
信号量也是进程间通信的方式 确保某一时刻只有一个进程能进入关键代码段 linux的信号量的API都定义在sys/sem.h中
主要包含三个系统调用 semget semop semctl被设计为操作一组信号量集
semget(key_t key,int num_sems,int sem_flags)
key是一个键值
num_sems参数指定信号量集中信号量的数目 创建信号量则该值必须被指定 如果是获取已经存在的信号量 则可以吧它设置为0
sem_flags参数指定一组标志 

semop系统调用改变信号量的值 就是执行PV操作 
semop(int sem_id,struct sembuf *sem_ops,size_t num_sem_ops)
sem_id是semget调用返回的信号量集标识符 用以指定被操作的目标信号量集 sem_ops指向一个sembuf结构体类型的数组 

semctl(int sem_id,int sem)num,int command
### 共享内存
#include<sys/shm.h>
共享内存是最高效的IPC机制 因为它不涉及进程之间的任何数据传输
这种高效率的问题是我们必须用其他的辅助手段来同步进程对共享内存的访问
4个系统调用 shmget shmat shmdt shmctl
int shmget(key_t key,size_t size,int shmflg) 
shmget系统调用创建一段新的共享内存 或者获取一段已经存在的共享内存
key来标识一段全局唯一的共享内存 size指定共享内存的字节 获取存在的共享内存时size设置为0
共享内存被创建/获取之后不能立即访问它 而是需要先将它关联到进程的地址空间中 使用完共享内存后我们需要将它从进程地址空间中分离
关联用void *shamt(int shm_id,const void *shm_addr,int shmflg) shm_id是由shmget调用返回的共享内存标识符 shm_addr参数指定将共享内存关联到进程的哪块地址空间 shm_addr推荐设置为NULL 由操作系统选择
分离用shmdt(const void *shm_addr)
### 消息队列 
#include<sys/msg.h>
消息队列是在两个进程之间传递二进制块数据的一种简单有效的方式 每个数据块都有特定的类型 接收方根据类型来有选择的接收数据 而不一定像管道和命名管道那样必须以先进先出的方式接收数据
int msgget(key_t key,int msgflg) 创建消息队列
int msgsnd(int msgid,const void *msg_ptr,size_t msg_sz,int msgflg) msgsnd把一条消息添加到消息队列中 msg_ptr指向一个准备发送的消息 
要发送的消息是一个结构体 struct msgbuf 里包含消息类型mtype 消息数据 mtext[512]数组 msgflg控制msgsnd的行为 支持以非阻塞的方式发送消息 如果发送消息的时候消息队列满了 msgsnd就阻塞
## 线程
#include<pthread.h>
创建线程pthread_create();
int pthread_create(pthread_t *thread,const pthread_attr_t *attr,void *(*start_routine),(void*) arg);
attr用于设置线程的属性 设置为NULL表示使用默认线程属性 start_routine指向线程将要运行的函数 arg为其参数
start_routine指向的要运行的函数在类里必须是静态static 返回值必须是void *
create成功返回0 失败返回错误码
线程一旦创建好 内核就可以调度内核线程来执行start_routine函数指针所指向的函数 
结束线程 void pthread_exit(void *retval) 通过retval参数向线程的回收者传递其退出信息 执行完不会返回到调用者 而且永远不会失败
linux线程有两种状态 joinable/unjoinable 线程是joinable状态时 线程函数自己返回退出时或pthread_exit时都不会释放线程所占用的资源 只有调用了pthread_join()后才会释放 若是unjoinable状态的线程在线程函数退出或pthread_exit时会被自动释放 unjoinable属性可以在pthread_create时设置 

pthread_join(pthread_t thread, void** retval) 一个进程中的所有线程都可以调用pthread_join()来回收其他可回收的线程
thread为要回收的目标线程标识符
retval参数是目标线程返回的退出信息 成功返回0 失败返回错误码
EDEADLK:可能引起死锁 比如两个线程相互针对对方调用pthread_join 或者线程对自身调用pthread_join
EINVAL: 目标线程不可回收 或已经有其他线程正在回收目标线程
ESRCH: 目标线程不存在

pthread_cancel(pthread_t pthread) 异常终止一个线程 即取消线程
```bash
void *syh(){
  cout<"多线程"<<endl
}
pthread_t tids;
int ret=pthread_create(&tids,NULL,syh,NULL);
```
用户创建的线程总数不能超过threads_max内核参数定义的值
int pthread_detach (pthread_t __th)
### 在多线程中调用fork函数
若一个多线程程序的某个线程调用了fork函数 那么新的进程是否将自动创建和父进程相同数量的线程呢 
答:否 新创建的子进程只拥有一个执行线程 就是调用fork的那个线程的复制 并且子进程将自动继承父进程中互斥锁的状态 
即 父进程中被加锁的互斥锁在子进程中也是被锁住的 
那么问题来了: 子进程不清楚从父进程继承来的互斥锁的状态 这个互斥锁可能加锁了 但不是由调用fork函数的那个线程锁住的 而是由其他线程锁住
若这种情况下 子进程再次对该互斥锁执行加锁操作就会导致死锁
函数pthread_atfork(void *(prepare)(void),void *parent,void *child) 会确保调用后父进程和子进程都有一个清楚的锁状态
prepare句柄将在fork调用创建出子进程之前被执行 它可以用来锁住所有父进程中的互斥锁 parent句柄则是fork调用创建出子进程之后 fork返回之前 在子进程中执行 child句柄用于释放所有在prepare句柄中被锁住的互斥锁 函数成功返回0 失败返回错误码
### 多线程下的信号
#include<pthread.h>
#include<signal.h>
int pthread_sigmask(int how, const sigset_t *newmask,sigset_t *oldmask);
由于进程中的所有线程共享该进程的信号 所以线程库将根据线程掩码决定把信号发送给哪个具体的线程 因此 若在每个子线程中都单独设置信号掩码 就容易产生逻辑错误 此外 所有线程共享信号处理函数 也就是说一个线程中设置了某个信号的信号处理函数 它将覆盖其他线程为同一个信号设置的信号处理函数 所以我们应该定义一个专门的线程来处理所有的信号
第一步 在主线程创建出其他线程之前就调用pthread_sigmask来设置好信号掩码 所有新创建的子线程都将自动继承这个信号掩码 这样之后所有线程都不会响应被屏蔽的信号了
第二步 在某个线程中调用int sigwait(const sigset_t *set, int *sig)
set参数指定需要等待信号的集合 即第一步创建的信号掩码 表示该线程中等待所有被屏蔽的信号 参数sig指向的整数用于存储该函数返回的信号值 sigwait成功时返回0 失败返回错误码 一旦sigwait正确返回我们就可以对接收到的信号做处理了 如果我们用了sigwait 就不应该在为信号设置信号处理函数了
pthread_kill(pthread_t thread,int sig) 将一个信号发送给指定的目标线程thread sig指定待发送的信号 sig=0时 则pthread_kill不发送信号 但仍会执行错误检查 我们可以用这种方式来检测目标线程是否存在 pthread_kill成功时返回0 失败返回错误码
## locker.h
### 信号量 class sem
#include<semaphore.h>
创建信号量sem_t sem
sem_t是一个union 
源代码原型为
```bash
typedef union
{
  char __size[__SIZEOF_SEM_T];//# define __SIZEOF_SEM_T	32
  long int __align;
} sem_t;
```
#### sem_init()
函数原型
```bash
int sem_init (sem_t *__sem, int __pshared, unsigned int __value);
```
用于初始化一个信号量sem 并给他一个初始整数值value pshared控制信号量的类型 pshared=0表示该信号量sem用于多线程的同步 pshared>0表示可以共享 用于多个进程间的同步(参数 pshared > 0 时指定了 sem 处于共享内存区域，所以可以在进程间共享该变量)
#### sem_destroy()
函数原型
```bash
int sem_destroy(sem_t *__sem);
```
用于销毁信号量sem
#### sem_wait()
函数原型
```bash
int sem_wait (sem_t *__sem);
```
sem_wait()是一个阻塞的函数 操作是原子操作 若sem的value>0 则value-1 并立即返回 若value=0 就一直阻塞到value>0
有个功能相近的函数 sem_trywait()是非阻塞的函数 会尝试获取sem的value值 如果sem的value=0 不阻塞 立即返回一个错误 值为EAGAIN
#### sem_post()
函数原型
```bash
int sem_post (sem_t *__sem);
```
操作是原子操作 把sem的value+1 并唤醒正在等待该信号量的任意线程
#### sem_getvalue()
```bash
int sem_getvalue (sem_t *__restrict __sem, int *__restrict __sval);
```
获取信号量的当前值 并将值保存在sbal中 若有一个或多个线程正在调用sem_wait()阻塞在此信号量上 该函数返回阻塞的进程/线程个数

### 互斥锁 class locker
互斥锁是信号量的一个特殊情况(n=1)
#include<pthread.h>
创建互斥锁pthread_mutex_t mutex;
互斥锁的类型：
1.普通锁 pthread_mutex_timed mutex加锁后其余请求的线程按照申请时间的顺序形成一个等待队列 
也就是第一次上锁成功 第二次上锁会阻塞
2.嵌套锁 pthread_mutex_recursive 也是递归锁
允许同一个线程对同一个锁获得多次 并可以解锁多次 如果是不同线程请求 就在加锁线程解锁时重新竞争
也就是第一次上锁成功 第二次上锁也成功
3.检错锁 pthread_mutex_errorcheck 如果线程在不解锁的状态下尝试重新上锁 会返回错误
也就是第一次上锁成功 第二次上锁失败
#### pthread_mutex_init()
```bash
int pthread_mutex_init (pthread_mutex_t *__mutex,const pthread_mutexattr_t *__mutexattr);
```
用于动态初始化互斥锁 参数attr指定了互斥锁的属性 若attr为NULL 则互斥锁属性为默认的快速互斥锁 不同属性的锁类型在试图对一个已经被锁定的互斥锁加锁表现不同 初始化成功后返回0 并把互斥锁mutex初始化为未被锁住的状态
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER 用于静态初始化互斥锁
#### pthread_mutex_destroy()
函数原型
```bash
int pthread_mutex_destroy (pthread_mutex_t *__mutex);
```
用于注销一个互斥锁mutex 释放mutex占用的资源 要求锁当前处于未被锁住的状态
#### pthread_mutex_lock()
```bash
int pthread_mutex_lock (pthread_mutex_t *__mutex)
```
上锁 执行成功返回0
若mutex已被锁住 调用这个函数的线程进入阻塞态 直到mutex可用为止
pthread_mutex_trylock()也可以用来上锁 不同在于 如果mutex已被其他线程占据锁住的时候并不挂起等待 而是返回EBUSY
#### pthread_mutex_unlock()
```bash
int pthread_mutex_unlock (pthread_mutex_t *__mutex)
```
解锁
#### pthread_
### 条件变量 class cond
若形容互斥锁是用于同步线程对共享数据的访问的话 那么条件变量则是用于在线程之间同步共享数据的值 条件变量提供了一种线程间的通知机制:当某个共享数据达到某个值的时候就唤醒等待这个共享数据的线程
#include<pthread.h>
pthread_cond_t cond 条件变量cond
#### pthread_cond_init()
```bash
int pthread_cond_init (pthread_cond_t *__restrict __cond,const pthread_condattr_t *__restrict __cond_attr)
```
初始化一个条件变量cond 参数attr是一个指针 指向类型为union的pthread_condattr_t 参数为空指针NULL时 表示函数创建的是默认的条件变量
函数执行成功返回0
#### pthread_cond_destroy()
销毁一个条件变量cond
```bash
int pthread_cond_destroy (pthread_cond_t *__cond)
```
#### pthread_cond_wait()
```bash
pthread_cond_wait (pthread_cond_t *__restrict __cond,pthread_mutex_t *__restrict __mutex)
```
用于等待目标条件变量 mutex是用于保护条件变量的互斥锁 确保wait操作的原子性 
调用pthread_cond_wait()前必须保证mutex已经加锁 执行时 首先把调用线程放入条件变量的等待队列中 然后将mutex解锁(这样pthread_cond_signal/broadcast不会修改条件变量) wait函数成功返回时 mutex会再被锁上

#### pthread_cond_broadcast()
以广播形式唤醒所有等待目标条件变量cond的线程
```bash
int pthread_cond_broadcast (pthread_cond_t *__cond);
```
#### pthread_cond_signal()
唤醒一个等待目标条件变量的线程 至于唤醒哪一个线程 取决于线程的优先级和调度策略
```bash
pthread_cond_signal (pthread_cond_t *__cond)
```
如何唤醒一个指定的线程(系统并未提供方法):定义一个能够唯一表示目标线程的全局变量 在唤醒等待条件变量的线程前先设置该变量为目标线程
然后采用广播的方式唤醒所有等待条件变量的线程 这些线程被唤醒后都检查该变量以判断被唤醒的是否是自己 如果是就开始执行后续代码 不是就返回继续等待

### 线程池 threadpool.h

### http_conn.h
#### HTTP报文结构
HTTP有两种报文
请求报文:客户向服务器发送请求报文
响应报文:服务器应答客户的报文
请求报文和响应报文都是由三个部分组成 区别是开始行不同
##### 开始行
用于区分请求报文和响应报文 请求报文中的开始行叫请求行 响应报文中的开始行叫状态行
HTTP请求报文由请求行 请求头部 空行 请求数据 四个部分组成
HTTP响应报文由状态行 消息报头 空行 相应正文 四个部分组成
请求行包含了请求方法+空格+URL+空格+HTTP版本+CRLF(回车换行)
状态行包含了HTTP版本+空格+状态码+短语+CRLF
##### GET请求报文
```bash
GET /562f25980001b1b106000338.jpg HTTP/1.1   #请求行
Host:img.mukewang.com                        #空行前都是请求头部
User-Agent:Mozilla/5.0 (Windows NT 10.0; WOW64) #HTTP客户端程序的信息，该信息由你发出请求使用的浏览器来定义,并且在每个请求中自动发送等
AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.106 Safari/537.36
Accept:image/webp,image/*,*/*;q=0.8 #说明用户代理可处理的媒体类型。
Referer:http://www.imooc.com/
Accept-Encoding:gzip, deflate, sdch          #说明用户代理可处理的媒体类型。
Accept-Language:zh-CN,zh;q=0.8               #说明用户代理能够处理的自然语言集。
空行
请求数据为空  #请求数据也叫主体
```
##### POST请求报文
```bash
POST / HTTP1.1 #请求行
Host:www.wrox.com  #空行前是请求头部
User-Agent:Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1; .NET CLR 2.0.50727; .NET CLR 3.0.04506.648; .NET CLR 3.5.21022)
Content-Type:application/x-www-form-urlencoded
Content-Length:40
Connection: Keep-Alive
空行
name=Professional%20Ajax&publisher=Wiley #请求数据
```
##### HTTP响应报文
```bash
HTTP/1.1 200 OK    #状态行 由HTTP版本号 状态码 状态消息 组成
Date: Fri, 22 May 2009 06:07:21 GMT  #二三行为消息报头 说明客户端要使用的信息
Content-Type: text/html; charset=UTF-8
空行
<html> #响应正文
<head></head>
<body>
<!--body goes here-->
</body>
</html>
```

#### http报文处理流程
浏览器端发出http连接请求 主线程创建http对象接收请求 并将所有数据读入对应buffer中(read_once函数) 将该对象插入任务队列 工作线程从任务队列中取出一个任务进行处理
工作线程取出任务后 调用process_read() 通过主/从状态机对请求报文进行解析
解析完成后 跳转到do_request()生成响应报文 并通过process_write()写入buffer 返回给浏览器端 

#### 状态机
主状态机对每一行数据进行解析 内状态机负责读取报文的每一行
##### 从状态机 parse_line()
process_read()驱动parse_line()检查报文语法 如果语法错误 返回Line_Bad 报文结束的最后有\r\n字符 检测到报文有\r\n就返回Line_Ok表示接收完毕 
然后通过循环来不停的返回状态 驱动主状态机解析报文request headers content
##### 主状态机
parse_request_line(char *text) 解析http报文的请求行 获得请求方法，目标url及http版本号 各个部分通过空格或\t隔开 解析完将m_check_state转移到分析http头部信息
parse_headers(char *text) 解析http请求报文的头部 包括Connection keep-live Content-length Host
parse_content(char *text) 解析http主体消息 因为get请求报文消息体是空的 所以解析消息体一定是post请求
解析完消息体就会跳转到do_request()报文响应 来进行检测登录和注册检测 标志位 *(p+1)
*(p+1)=3就跳转到注册页面log.html 先检测数据库中是否有重名 如果没有就增加数据 如果重名就跳转到registerError.html页面提示重复注册
*(p+1)=2就是登录 若浏览器端输入的用户名和密码能被查询到 就返回1 跳转到welcome页面 查询不到返回0调转到logError.html页面

##### 响应报文处理
无论是从状态机给出Bad_Request还是解析完报文进入do_request() 服务器子线程最后都会进入process_write() process_write包括add_status_line(状态行) add_headers(消息报头) add_blank_line(空行) m_file_adress(响应正文) 响应报文写入iovec[] 主线程通过write()返回响应报文发送到浏览器端

### lst_timer.h 定时器