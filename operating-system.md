# 锁

**互斥锁(悲观)**

当一个线程获得`互斥锁`，另一个线程想要获得该锁会失败，进入睡眠等待唤醒

**自旋锁(悲观)**

当一个线程获得`自旋锁`，另一个线程想要获得该锁会失败，一致循环是否能获得该锁

**读写锁(悲观)**

一个`读写锁`同时只能有一个写者或多个读者

`读写锁`当前没有读者或写者，写者可以立刻获得`读写锁`，否则自旋

`读写锁`没有写者，读者可以立刻获得`读写锁`，否则自旋

**CAS(乐观)**

由硬件实现

主要三个参数：新变量，预期的变量，更改前的变量

当预期的变量等于更改前的变量，才用新变量赋值

**偏向锁(乐观)**

`偏向锁`会偏向于第一个获得它的线程，当这个线程再次请求锁时不需要进行任何同步操作，**其他线程请求该锁会导致锁升级**

## 锁优化

- 减少锁持有时间：不需要锁的操作就不要上锁
- 较少锁粒度：将锁细分，防止多个线程争抢同一个锁
- 锁分离：分为不同类型的锁，例如读写锁
- 锁粗话：减少锁频繁请求释放
- 锁消除：编译器判断是否会竞争，做到不上锁

# 死锁

**定义**

是指两个或两个以上的进程在执行过程中，因争夺资源而造成的一种互相等待的现象，若无外力作用，它们都将无法推进下去

**条件**

- 互斥条件: 一个资源每次只能被一个进程使用
- 请求和保持: 一个进程因请求资源而阻塞时,对已获得的资源保持不放
- 不可抢占: 进程已获得的资源,在未使用完之前,不能强行剥夺
- 循环等待: 若干进程之间形成一种头尾相接的循环等待资源关系

这四个条件是死锁的必要条件，只要系统发生死锁，这些条件必然成立，而只要上述条件之
一不满足，就不会发生死锁

# 进程的状态

创建状态、就绪状态、执行状态、阻塞状态、终止状态

# (IPC)进程间的通信

管道。a 进程给 b 进程传输数据，只能等待 b 进程取了数据之后 a 进程才能返回；

消息队列。消息队列提供暂存点；

共享内存。两进程各自拿出一块虚拟地址空间，映射相同的物理内存；

信号量。资源竞争表现；

Socket。



# 内核态和用户态

用户态执行，进程所访问的内存空间和对象受到限制，其占有的处理器是可被抢占的

内核态执行，进程能访问所有的内存空间和对象，且占有的处理器是不可被抢占的

用户态切换到内核态方式：

- 系统调用（主动）：用户态进程调用调用系统提供的服务
- 异常（被动）：用户态进程遇到不可知异常，就会触发进程切换到处理此异常的内核相关程序
- 外围设备中断（被动）：外围设备完成发出的中断

# 线程同步方式

互斥量。如锁

信号量。

事件

# 进程同步方式

临界区、互斥量、信号量、事件

# 进程调度算法

先到先服务调度算法

短作业优先调度算法

时间片轮转调度算法

多级反馈队列调度算法

优先级调度

# 内存管理主要做什么

负责内存的分配和回收

将逻辑地址转化为物理地址

# 页面置换算法

最佳页面置换算法

先进先出页面置换算法

最近最久未使用页面置换算法

最少使用页面置换算法

# 局部性原理

时间局部性

如果一个信息项正在被访问，那么在近期它很可能被再次访问

空间局部性

如果一个存储器的位置被引用，那么将来他附近的位置也会被引用

# 管程

管程是一种高级的同步原语。

**重要特性：任意时刻管程中只能有一个活跃进程。**

它是一种编程语言的组件，所以编译器知道它们很特殊

一个管程包含:

- 多个彼此可以交互并共用资源的[线程](http://zh.wikipedia.org/wiki/线程)
- 多个与资源使用有关的变量
- 一个[互斥锁](http://zh.wikipedia.org/wiki/互斥锁)
- 一个用来避免[竞态条件](http://zh.wikipedia.org/wiki/競態條件)的[不变量](http://zh.wikipedia.org/wiki/不變量)