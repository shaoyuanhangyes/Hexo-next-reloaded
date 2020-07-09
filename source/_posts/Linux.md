---
title: Linux
date: 2020-02-07 12:30:20
categories:
- Linux
tags:
- Linux
- Ubuntu
- Server
mathjax: true
---
## 自用Linux命令笔记
<!-- more -->
## 命令 
ls 查看当前目录下的文件夹有哪些
mkdir 新建一个文件夹
pwd 显示当前文件夹
rm 删除
cp 拷贝文件
mv 移动文件
tar 压缩文件
unzip 解压zip文件
ps 显示系统的运行进程
top 显示cpu占用
# Linux下多线程编程
## 服务器编程基本框架
由I/O处理单元 逻辑单元 网络存储单元(可选)组成 每个单元间通过请求队列进行通信来协作
I/O处理单元:处理客户端连接 读写网络数据
逻辑单元:用于处理业务逻辑的线程
网络存储单元:本地数据库和文件等 

## 五种I/O模型
应用程序向操作系统发出I/O请求:应用程序发出IO请求给操作系统内核 操作系统内核需要等待数据就绪 这里的数据可能来自别的应用程序或者网络
一般I/O分为两个阶段 等待数据阶段 拷贝数据阶段 等待数据阶段 若没数据 应用程序就阻塞等待 拷贝数据阶段是将就绪的数据拷贝到应用程序工作区
在Linux系统中 操作系统的I/O操作是一个系统调用recvfrom() 就是一个系统调用recvfrom包含等待数据就绪和拷贝数据
### I/O
输入输出(input/output)的对象可以是文件(file) 网络(socket) 进程之间的管道(pipe)
在linux系统中 都用文件描述符(fd)来表示
同步/异步是真对应用程序和内核的交互而言的 同步指用户进程触发I/O操作并等待或者轮询的去查看I/O操作是否就绪 比如自己洗衣服的时候不能干别的事情
异步是指用户进程触发I/O操作之后便开始做其他的事情 当I/O操作已经完成的时候会得到I/O操作完成的通知 例如自己用洗衣机洗衣服 期间去做别的事情 洗衣机洗好衣服会有声音提示

阻塞/非阻塞是针对进程在访问数据的时候 根据I/O操作的就绪状态来采取的不同方式
阻塞指当试图对文件描述符进行读写时 若缓冲区没东西可读或暂时不可写 程序就进入等待状态 直到有东西可读/写为止 比如人工窗口充值 若充值业务员不在 那我们只能在原地等待 一直等到业务员回来为止
非阻塞指当试图对文件描述符进行读写时 若缓冲区没东西可读或暂时不可写 读写函数马上返回 不会等待 比如在银行排队办业务 领取一张呼号票 之后去干自己的事情 等轮到我们了银行会呼叫通知 这时候我们再去办业务
### 事件
1. 可读事件:当文件描述符关联的内核读缓冲区可读 则触发可读事件 (可读：内核缓冲区非空，有数据可以读取)
2. 可写事件:当文件描述符关联的内核写缓冲区可写 则触发可写事件 (可写：内核缓冲区不满，有空闲空间可以写入）


### 通知机制
就是当事件发生时主动通知 通知机制的反面是轮询机制 意思就是挨个遍历去询问才能得到消息

### epoll
epoll是一种I/O事件通知机制 是linux内核实现I/O多路复用的一个实现 
I/O多路复用是指在一个操作里同时监听多个输入输出源 在其中一个或多个输入输出源可用的时候返回 然后对其进行读写操作
简言之 epoll是一种当文件描述符的内核缓冲区非空的时候发出可读信号来通知 当写缓冲区不满的时候发出可写的信号通知的一种机制
#### epoll API
##### int epoll_create(int size)
内核产生一个epoll实例数据结构并返回一个文件描述符(fd) 这个描述符就是epoll实例的句柄 后面的两个接口都以它为中心(形参epfd)
size表示所要监视文件描述符的最大值 不过后来的linux版本中已经被弃用 同时size不能传0 会抛出 invalid argument的error

##### int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event)
将被监听的描述符添加到红黑树或从红黑树中删除或者对监听事件进行修改
```bash
typedef union epoll_data{
    void *ptr; //指向用户自定义数据
    int fd; //注册的文件描述符
    unint32_t u32; //32bit integer
    uint64_t u64; //64bit integer
} epoll_data_t;

struct epolll_event{
    uint32_t events; //描述epoll事件
    //events域描述一组epoll事件 在epoll_ctl调用中解释为描述符所期望的epoll事件 可多选

    epoll_data_t data; //上面的union 
    //data域是唯一能给出描述符信息的字段 所以在调用epoll_ctl加入一个需要检测的描述符时
    //一定要在data域写入描述符相关信息
}
```

对于需要监听的文件描述符的集合 epoll_ctl对红黑树进行管理 红黑树中的每个成员由描述符的值和所要监控的文件描述符指向的文件表项的引用等组成

op参数的操作类型有:
    1. epoll_ctl_add 向interest list添加一个需要监视的描述符
    2. epoll_ctl_del 从interest list中删除一个描述符
    3. epoll_ctl_mod 修改interest list中的一个描述符

struct epoll_event结构描述一个文件描述符的epoll行为 在使用epoll_wait()返回处于ready状态的描述符列表

常用epoll事件描述:
     epollin 描述符处于可读状态
     epollout 描述符处于可写状态
     epollpri 由带外数据触发 表示描述符有紧急的数据可读
     epollet 将epoll event通知模式设置为edge triggered 设置为边缘触发
     epolloneshot 第一次进行通知 之后不再监测 如果还想继续监测 就需要再次把这个socket加入到epoll队列中
     epollhup 本端描述符产生一个挂断事件 默认监测事件
     epollrdhup 对端描述符产生一个挂断事件
     epollerr 描述符产生错误时触发 默认监测事件
     

##### int epoll_wait(int epfd, struct epoll_event *events, int maxevents, int timeout)
阻塞等待注册的事件发生 返回事件的数目 并将触发的事件写入events数组中
epoll_wait返回值number是不会大于maxevents的 返回的是活跃客户端的数量 并且将这些活跃的客户端信息加入到events
events:用来记录被触发的events 大小和maxevents一致
maxevents:返回events的最大个数

处于ready状态的那些文件描述符会被复制进ready list中 epoll_wait()用于向用户进程返回ready list
events和maxevents两个参数描述一个由用户分配的struct epoll event数组 调用返回时 内核将ready list复制到这个数组中 并将实际复制的个数作为返回值
如果ready list比maxevents长就只能复制maxevents个成员

timeout:描述函数调用中阻塞时间上限 单位ms
    1. timeout = -1 表示调用将一直阻塞 直到有文件描述符进入ready状态或者捕捉到信号才返回
    2. timeout = 0  用于非阻塞检测是否有描述符处于ready状态 不管结果如何 调用都立即返回
    3. timeout > 0  表示调用将最多持续timeout时间 如果期间有检测对象变为ready状态或捕捉到信号则返回 否则直到超时

#### epoll的两种触发方式
epoll监控多个文件描述符的I/O事件 epoll支持边缘触发(edge trigger ET)和水平触发(level trigger LT) 通过epoll_wait()等待I/O事件
如果当前没有可用事件就阻塞调用线程

select和poll只支持LT工作模式 epoll的默认工作模式是LT模式
##### LT水平触发的时机
对于读操作 只要缓冲区不为空 LT模式返回读就绪
对于写操作 只要缓冲区还没满 LT模式返回写就绪
当被监控的文件描述符上有可读写事件发生时 epoll_wait()会通知处理程序去读写 如果这次没有把数据一次性全读完(读写缓冲区太小)
那么下次调用epoll_wait()时 它还会通知你在上次没读写完的文件描述符上继续读写 若你一直不去读写 它会一直通知你
若系统中有大量你不需要读写的文件描述符 而他们每次都返回 会大大降低处理程序检索自己关心的文件描述符的效率

##### ET边缘触发的时机
对于读操作 
    1. 当缓冲区由不可读变为可读的时候(缓冲区由空变为不空的时候)
    2. 当有新数据到达的时候(缓冲区中的待读数据变多的时候)
    3. 当缓冲区内有数据可读 且应用程序对相应的文件描述符进行epoll_ctl_mod修改epollin事件时

对于写操作
    1. 当缓冲区由不可写变为可写时
    2. 当有旧数据被发送走 即缓冲区种数据变少的时候
    3. 当缓冲区有空间可写 且应用进程对相应的文件描述符进行epoll_ctl_mod修改epollout事件时

当被监控的文件描述符上有可读写事件发生时 epoll_wait()会通知处理程序去读写 如果这次没有把数据一次性全读完(读写缓冲区太小)
那么下次调用epoll_wait()时 那么下次调用epoll_wait()时 也就是它只会通知你一次 直到该文件描述符上出现第二次可读写事件才会通知你
这种模式比水平触发效率高 系统不会充斥大量你不关心的就绪文件描述符

在边缘触发的模式下 缓冲区从不可读变成可读 会唤醒应用进程 缓冲区数据变少的情况下不会再唤醒应用进程

For instance 1):
    1. 读缓冲区刚开始是空的
    2. 读缓冲区写入2KB数据
    3. 水平触发和边缘触发此时都会发出可读信号
    4. 收到信号通知后读取了1kb的数据 缓冲区还剩1kb
    5. 水平触发会再通知你继续读写 边缘触发不会再通知你

For instance 2):
    水平触发:0表示缓冲区无数据 1表示缓冲区有数据 缓冲区有数据则一直为1 就一直触发
    边缘触发:0表示缓冲区无数据 1表示缓冲区有数据 只有在0变为1的时候才触发

#### epoll select poll比较

##### 用户态将文件描述符传入内核的方式
select: 创建3个文件描述符并拷贝到内核中 分别监听读/写/异常 这里单个进程可以打开的文件描述符fd数量默认限制为1024
poll: 将传入的struct pollfd结构体数组拷贝到内核中进行监听
epoll: 执行epoll_create会在内核的高速cache区域中建立一棵红黑树以及就绪链表(该链表存储已经就绪的文件描述符) 用户执行的epoll_ctl()添加文件描述符会在红黑树上增加相应的结点

##### 内核态检测文件描述符读写状态的方式
select: 采用轮询方式 遍历所有fd 最后返回一个描述符读写操作是否就绪的mask掩码 根据这个掩码给fd_set赋值
poll: 采用轮询方式 查询每个fd的状态 若就绪则在等待队列中加入一项并继续遍历
epoll: 采用回调机制 执行epoll_ctl的add操作时 不仅将文件描述符放进红黑树中 而且也注册了回调函数 内核在检测到某文件描述符可读/可写的时候会调用回调函数 该回调函数将fd放在就绪链表中

##### 找到就绪的文件描述符并传递给用户态的方式
select: 将之前传入的fd_set拷贝传出到用户态并返回就绪的文件描述符总数 用户态并不知道是哪些文件描述符处于就绪态 需要遍历来判断
poll: 将之前传入的fd数组拷贝传出用户态并返回就绪的文件描述符总数 用户态并不知道是哪些文件描述符处于就绪态 需要遍历来判断
epoll: epoll_wait()只用观察就绪链表中有无数据就行 最后将链表的数据返回给数组并返回就绪的数量 内核将就绪的文件描述符放在传入的数组中 所以只用遍历依次处理即可 这里返回的文件描述符是通过mmap让内核和用户空间共享同一块内存实现的 减少了不必要的拷贝

##### 重复监听的处理方式
select: 将新的监听文件描述符集合拷贝传入内核中 继续以上步骤
poll: 将新的struct pollfd结构体数组拷贝传入内核中 继续以上步骤
epoll: 无需重新构建红黑树 直接沿用已存在的即可

#### epoll比select/poll更高效的原因
1.select采用fd标注位来存放 poll采用链表来进行文件描述符的存储 所以select会受到最大连接数的限制 poll就不会
2.select poll epoll虽然都会返回就绪的文件描述符数量 但select/poll并不会明确指出哪些fd处于就绪状态 epoll可以 系统调用返回后 调用select/poll的程序需要遍历监听的整个fd来找到谁处于就绪 epoll可以直接处理
3.select/poll都需要将有关文件描述符的数据结构拷贝进内核 最后再拷贝出来 而epoll创建的有关文件描述符的数据结构本身就存在内核态中 epoll使用mmap()减少复制开销
4.select/poll采用轮询方式来检查fd是否处于就绪态 epoll采用回调机制 随着fd的增加 select/poll的效率线性降低 epoll不会受到太大影响 除非活跃的socket太多
5.epoll的边缘触发机制效率高 系统不会充斥大量的不关心的就绪文件描述符
假如连接数少并且连接都十分活跃情况下 select/poll性能比epoll好 因为epoll的通知机制需要很多函数回调 

### 四种同步I/O
同步I/O指内核向应用程序通知的是就绪事件 比如只通知有客户端连接 要求用户代码自行执行I/O操作

#### 同步阻塞I/O
用户进程在发起一个I/O操作以后必须等待I/O操作的完成 只有I/O操作完成之后 用户进程才能运行

#### 同步非阻塞I/O
用户进程发起一个I/O操作后就返回做其他事情 每隔一段时间就去检查I/O事件是否就绪 没有就绪就可以做其他事 非阻塞I/O执行系统调用总是立即返回 不管时间是否已经发生 若时间没有发生 则返回-1
此时可以根据errno区分这两种情况 对于accept recv send 事件未发生时 errno被设置成eagain

#### 信号驱动I/O
应用程序发起I/O请求时 给I/O安装一个信号处理函数 请求立即返回 操作系统底层则处于等待状态(等待数据就绪) 直到数据就绪收到SIGIO信号 然后通过信号通知主调程序 主调程序采取调用系统函数recvfrom()完成I/O操作


#### 多路复用I/O
也称之为事件驱动I/O linux用 select/poll函数实现I/O复用模型 select/poll好处为单个进程就可以同时处理多个网络连接的I/O
基本原理是select/poll会不断的轮询遍历所有socket 当某个socket有数据到达了就通知用户进程
多路复用中 通过select可以同时监听多个I/O请求的内核操作 只要有任意一个内核操作就绪 都可以通过select返回 再进行系统调用recvfrom()完成I/O操作
这个过程应用程序就可以同时监听多个I/O请求 比基于多线程阻塞I/O好很多 因为服务器只需要少数线程就可以进行大量客户端通信
但是和阻塞I/O不同的是这两个函数可以同时阻塞多个I/O操作 而且可以同时对多个读/写操作的I/O函数进行检测 知道有数据可读或可写时才真正调用I/O操作函数
### 异步I/O
异步I/O是指内核向应用程序通知的是完成事件 比如读取客户端的数据后才通知应用程序 由内核完成I/O操作
linux中 可以调用aio_read()告诉内核描述字缓冲区指针和缓冲区的大小 文件偏移及通知的方式 然后立即返回 当内核将数据拷贝到缓冲区后在通知应用程序
整个过程中应用程序不需要阻塞

### I/O模型举例
想吃牛肉面
同步阻塞: 在饭店点餐 然后在那坐着等着 还要不停的问做好了没有
同步非阻塞: 在饭店点完餐 就出去溜达了 不过溜达一会就回来问做好了没有
多路复用: 溜达的时候 饭店打电话说饭做好了来自取一下
异步(非阻塞): 饭店打电话说 面做好了我现在送到你所在的地方

## BIO
在读取输入流或者写入输出流时 在读/写操作完成之前 线程会一直阻塞 传统的服务器端同步阻塞I/O 
## NIO
基于事件驱动思想 采用reactor模式 是一种同步非阻塞的I/O模型 也是I/O多路复用的基础 成为解决高并发与大量连接的有效方式
I/O处理单元只负责监听文件描述符上是否可读/写 有的话由操作系统通知应用程序 应用程序再将流读取到缓冲区或写入系统
NIO适合连接数目多连接比较短的架构 例如聊天服务器 
## AIO
基于事件驱动思想 采用Proactor模式 是一种异步I/O模型 进行I/O操作时 直接调用API的read和write 对于读操作 操作系统将数据读到缓冲区 兵通知应用程序 对于写操作 操作系统将write()传递的流写入并主动通知应用程序 节省了NIO中select轮询遍历通知队列的代价
AIO的实施需要充分调用OS参与 所以性能方面不同操作系统差异比较明显 因此实际中AIO应用不广泛
AIO适用于连接数目多且连接比较长的架构 例如相册服务器 AIO接受数据需要预先分配缓存 对连接数量大流量小的情况造成了内存浪费

## Reactor模式 
要求主线程(I/O处理单元)只负责循环等待监听文件描述符是否有事件发生 有的话就通知工作线程(逻辑单元) 除此之外主线程不做任何其他实质性工作
读写数据 接受新的连接 处理客户请求 都在工作线程中完成
使用同步I/O模型(epoll_wait为例)实现Reactor模式的流程:
1.主线程往epoll内核事件表中注册socket上的读就绪事件
主线程调用epoll_wait等待socket上有数据可读
当socket上有数据可读时 epoll_wait通知主线程 主线程将socket可读事件插入请求队列
睡眠在请求队列上的某个工作线程被唤醒 它从socket读取数据 并处理客户请求 然后往epoll内核事件表中注册该socket上写就绪事件
主线程调用epoll_wait等待socket可写
当socket可写的时候 epoll_wait通知主线程 主线程将socket可写事件插入请求队列
睡眠在请求队列上的某个工作线程被唤醒 它往socket上写入服务器处理客户请求的结果

<div style="width: 600px;height: 200px;">
    ![Reactor](Reactor.png)
</div>

工作线程从请求队列中取出事件后 将根据事件的类型来决定如何处理: 对于可读事件 执行读数据和处理请求的操作 对于可写事件 执行写数据的操作

## Proactor模式
与Reactor模式不同 Proactor模式将所有I/O操作都交给主线程和内核来处理 工作线程只负责业务逻辑

### 使用同步I/O方式模拟出Proactor模式
原理: 主线程执行数据读写操作 读写完成后 主线程向工作线程通知这一"完成事件" 从工作线程的角度看 它直接获得了数据读写的结果 只需要对读写结果进行逻辑处理
流程:
1.主线程往epoll内核事件表中注册socket上的读就绪事件
主线程调用epoll_wait等待socket上有数据可读
当socket上有数据可读时 epoll_wait通知主线程 主线程从socket循环读取数据 直到没有数据可读 然后将读取的数据封装成一个请求对象并插入请求队列
睡眠在请求队列上的某个工作线程被唤醒 它将获得请求对象并处理客户请求 然后往epoll内核事件表中注册该socket上写就绪事件
主线程调用epoll_wait等待socket可写
当socket可写的时候 epoll_wait通知主线程 主线程往socket上写入服务器处理客户请求的结果

<div style="width: 600px;height: 400px;">
    ![Proactor](Proactor.png)
</div>

## 半同步/半异步模式
并发模式中的同步异步是指 同步指程序按照代码序列的顺序执行 异步指程序的执行需要系统事件驱动 系统事件有中断 信号等
像服务器这种既要求实时性 有要求能同时处理多个客户请求 应该同时使用同步线程和异步线程 即采用半同步半异步模式实现
同步线程处理客户逻辑(相当于工作线程) 异步线程处理I/O事件(相当于主线程)  异步线程监听到客户请求后 将其封装成请求对象并插入请求队列中 请求队列将通知某个工作在同步模式的工作线程唉读取处理该请求对象




## 相关问题
1.线程数不多 多客户端请求
若是proactor模式 主线程读取完数据才能加入请求队列 这时候可以换成普通reactor 每个线程去接收数据 进一步可以使用主从reactor 
主从reactor中 主reactor是一个线程 负责监听连接请求 然后派发给accepor 传递给从reactor处理读写事件 处理请求的是其余N个不同线程 也就是从reactor 然后通过请求的密集度来调控reactor的个数
2.reactor模式中若一个iancheng被占用很久 服务器怎么解决接下来的连接请求
若没有线程数限制 目前项目设置是固定线程数 可以改成动态线程池
3.定时器是基于升序链表 换成时间轮呢
性能上提升有限 逻辑上更优 时间轮或者时间堆目的是时间上更加精准 将之前的自定义时间变成了当前最先超时的定时器与当前的时间差
现在的升序链表是自定义时间出发alarm新号 这样会造成遍历和触发的浪费 比如某次遍历发现没有任何超时要处理的定时器

## 高级I/O函数
和网络编程相关的三类高级I/O函数 用于创建文件描述符的pipe dup函数 用于读写的readv/writev sendfile mmap/munmap splice tee函数 用于控制I/O行为的fcntl函数
### pipe函数
pipe函数用于创建管道 来进行进程间通信 
```bash
#include<unistd.h>
int pipe(int fd[2]);
```
参数是一个包含两个int整数的数组指针 成功返回0 并将一对打开的文件描述符值填入参数指向的数组 失败返回-1并设置errno
pipe函数创建的两个文件描述符fd[0] fd[1]分别构成管道两端 fd[1]写入的数据可以从fd[0]读出 fd[0]只能从管道读数据 fd[1]只能往管道写数据 若要实现双向传输 应该使用两个管道 
默认情况下 这一对文件描述符都是阻塞的 若用read来读取一个空管道 read会被阻塞直到管道内有数据可读 若用write往一个满的管道写入数据 则write也会被阻塞 直到管道有空间可用
应用程序可以把fd[0] fd[1]设置为非阻塞的 
管道内部传输的数据是字节流 管道容量大小默认65536字节 可以使用fcntl函数来修改管道容量
```bash
#include<sys/types.h>
#include<sys/socket.h>
int socketpair(int domain, int type, int protocol, int fd[2]);
```
socketpair创建双向管道 domain只能是本地的协议簇 意味着我们仅能在本地使用这个双向管道 socketpair创建的fd[0] fd[1]既可读又可写
成功返回0 失败返回-1 并设置errno
### dup/dup2函数
```bash
#include<unistd.h>
int dup(int file_descriptor);
int dup2(int file_descriptor_one,int file_descriptor_two);
```
dup()创建一个新的文件描述符 和file_descriptor指向相同的文件管道网络连接 并且dup返回的文件描述符总是取系统当前可用的最小整数值
dup2将返回第一个不小于file_descriptor_two的整数值 
两个函数调用失败返回-1 并设置errno
### readv()/writev()
readv()将数据从文件描述符读到分散的内存块中 writev()将多块分散的内存数据一并写入文件描述符
```bash
#include<sys/uio.h>
ssize_t readv(int fd, const struct iovec *vector, int count)
ssize_t writev(int fd, const struct iovec *vector, int count)
``
fd参数是被操作的目标文件描述符 vector是类型是iovec的结构数据 count是vector的长度 
两个函数成功屎返回读出写入的fd字节数 失败返回-1 并设置errno
```
### sendfile()
```bash
#include<sys/sendfile.h>
ssize_t sendfile(int out_fd,int in_fd, off_t *offset, size_t count);
```
in_fd是待读出内容的文件描述符 out_fd是待写入内容的文件描述符 offset是指定读入文件流从哪个位置开始读 若为空就使用读入文件流的默认起始位置 count指定in_fd out_fd之间传输的字节数 
sendfile成功返回传输的字节数 失败返回-1并设置errno in_fd必须指向真实的文件 out_fd必须是一个socket
### mmap()/munmap()
mmap()用于申请一段内存空间 这段内存作为进程通信的`共享内存`
也可以将文件直接映射到其中 munmap()则释放由mmap创建的这段内存
```bash
#include<sys/mman.h>
void *mmap(void *start, size_t length, int port, int flags, int fd, off_t offset);

int munmap(void *start, size_t length);
```
start参数允许用户使用某个特定的地址作为这段内存的起始地址 被设置NULL就自动分配一个 length为这段内存的长度 port设置内存段的访问权限 有可读(PORT_READ) 可写(PORT_WRITE) 可执行(PORT_EXWC) 不可访问(PORT_NONE)
flags参数控制内存段内容被修改后程序的行为
fd参数是被映射文件对应的文件描述符 一般通过open()获得
offset参数设置文件从何处开始映射
mmap()成功返回指向目标内存区域的指针 失败返回MAP_FAILED((void*)-1) 并设置errno munmap()成功返回0 失败返回-1 并设置errno

### splice()
splice()在两个文件描述符之间移动数据(零拷贝操作)
```bash
#include<fcntl.h>
ssize_t splice(int fd_in, loff_t *off_in, int fd_out, loff_t *off_out, size_t len, unsigned int flags)
```
fd_in参数是待输入数据的文件描述符 若fd_in是管道文件描述符 那么off_in必须设置为NULL
若fd_in是socket off_in表示从输入数据流的何处开始读取数据 off_in被设置成NULL时表示从输入数据流的当前偏移位置读入 若不是NULL 则指出具体偏移位置
fd_out/off_out与fd_in/off_in相同 用于输出数据流 len指定移动数据的长度 flags参数控制数据如何移动
使用splice()时 fd_in/fd_out必须至少有一个是管道文件描述符 调用成功返回移动数据的字节 可能返回0表示没有数据要移动 失败返回-1 并设置errno
### tee()
```bash
#include<fcntl.h>
ssize_t tee(int fd_in, int fd_out, size_t len, unsigned int flags);
```
tee()在两个管到文件描述符之间复制数据 也是零拷贝操作不消耗数据 tee成功返回负值的字节数 可能返回0表示没有数据要移动 失败返回-1 并设置errno

### fcntl()
提供了对文件描述符的各种操作控制
```bash
#include<fcntl.h>
int fcntl(int fd, int cmd, ...);
```
fd是被操作的问价鸟舒服 cmd参数指定执行何种类型操作 根据操作类型不同 可能还需要第三个可选参数arg
<div style>
<h4 align="center">fcntl支持的常用操作和参数</h4>
    ![Fcntl1](fcntl.png)
</div>
fcntl失败返回-1 并设置errno
在网络编程中fcntl通常用来将一个文件描述符设置为非阻塞的

```bash
int setnonblocking(int fd){
    int old_option=fcntl(fd,F_GETFL);/*获取文件描述符旧状态*/
    int new_option=old_option | O_ONBLOCK; /*设置非阻塞标志*/
    fcntl(fd,F_SETFL, new_option);
    return old_option
}
```
## Linux系统日志

rsyslogd既能接收用户进程输出的日志 又能接收内核日志 用户进程通过调用syslog函数生成系统日志 该函数将日志输出到一个UNIX本地域socket类型(AF_UNIX)的文件/dev/log中 rsyslogd则监听文件获取用户进程输出  内核日志由printk打印到内核的环状缓存中 环状缓存内容映射到/proc/kmsg文件中 rsyslogd通过读取该文件获得内核日志
<div style>
<h4 align="center">Linux系统日志</h4>
    ![Linux系统日志](Linux系统日志.png)
</div>

### syslog()
```bash
#include<syslog.h>
void syslog(int priority, const char *message,...);
```
priority参数是设施值与日志级别的按位或 设施值的默认值为LOG_USER  日志级别有七种 分别是
LOG_EMERG 0 系统不可用 LOG_ALERT 1 报警 LOG_CRIT 2 非常严重 LOG_ERR 3 错误 LOG_WARNING 4 警告
LOG_NOTICE 5 通知 LOG_INFO 6 信息 LOG_DEBUG 7 调试
void openlog(const char *ident, int logopt, int facility)可以改变syslog默认输出方式
ident指定的字符串会被添加到日志消息的日期时间之后 通常被设置为程序名字 logopt对后续syslog调用的行为进行配置 
facility用来修改syslog函数中的默认设施值
为了过滤日志留下重要的调试信息 需要简单的设置日志掩码 日志级别大于掩码的日志信息会被系统忽略
int setlogmask(int maskpri) maskpri指定日志掩码值 返回进程之前的掩码值
最后不要忘了closelog();

### 进程间关系
#### 进程组
进程除了pid外 还有进程组的pgid
```bash
#Include<unistd.h>
pid_t getpgid(pid_t pid);//成功返回pid所属的pgid 失败返回-1
int setpgid(pid_t pid, pid_t pgid);//用于讲PID为pid的进程的PGID设置为pgid 若pid和pgid相同 则由pid指定的进程为进程组首领 pid=0表示当前进程的PGID为pgid 若pgid=0 则使用pid座位目标pGID 成功返回0 失败返回-1
```
一个进程只能设置自己或者其子进程的PGID 并且子进程调用exec函数后 不能在父进程中对它设置PGID
#### 会话 
一些有关联的进程会形成一个会话
```bash
#include<unistd.h>
pid_t setsid();//创建一个会话session
```
该函数不能由首领进程调用 否则产生错误
调用该函数不仅创建新会话 调用该函数的进程会成为会话的首领 此时该进程是新会话的唯一成员 新建一个进程组 它PGID就是调用进程的PID 调用进程成为该组的首领 调用进程甩开终端
成功返回新进程组的PGID 失败返回-1
Linux并未提供会话ID (SID)的概念 但Linux认为SID等于会话首领所在的进程组的PGID 
pid_t getsid(pid_t pid)来读取SID
#### PS命令查看进程 进程组 会话之间的关系
