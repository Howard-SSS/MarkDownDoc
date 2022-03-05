# APP生命周期

**5种状态**

not running：没启动

inactive：前台运行但不接收事件，如锁屏、电话来电

active：前台运行并接受事件

backgroud：后台运行并执行代码，时间长会suspended

suspended：开始时程序保留在内存，内存不足会被清除

![img](https://images0.cnblogs.com/blog/320167/201502/051500183431183.png)

**状态转变回调**

`application:willFinishLaunchingWithOptions:` - 这个方法是你在启动时的第一次机会来执行代码

`application:didFinishLaunchingWithOptions:` - 这个方法允许你在显示app给用户之前执行最后的初始化操作

`applicationDidBecomeActive:` - app已经切换到active状态后需要执行的操作

`applicationWillResignActive:` - app将要从前台切换到后台时需要执行的操作

`applicationDidEnterBackground:` - app已经进入后台后需要执行的操作

`applicationWillEnterForeground:` - app将要从后台切换到前台需要执行的操作，但app还不是active状态

`applicationWillTerminate:` - app将要结束时需要执行的操作

# ViewController生命周期

initWithCoder(类初始化)

init(对象初始化)

loadView：UIViewController的view被访问且为空时调用，实例化UIViewController的view属性

viewDidLoad

viewWillAppear

viewWillLayoutSubviews

viewDidLayoutSubviews

viewDidAppear

viewWillDisappear

viewDidDisappear

# View生命周期

init

didAddSubview

willMoveToSuperview：视图即将加入或移除时调用

DidMoveToSuperview

willMoveToWindow：视图即将加入或移除时调用

didMoveToWindow

layoutSubviews

drawRect

removeFromSuperview

dealloc

willRemoveSubview

# KVO原理

当观察某对象A时，KVO机制动态创建一个对象A当前类的子类，并为这个新的子类重写了被观察属性keyPath的setter方法。setter方法随后负责通知观察对象属性的改变状况

# KVC

键值编码，使用字符串访问对象的属性

 当通过setValue:forKey:赋值时，其底层流程为：

 1、查找是否有setName，_setName，setIsName的set方法，如果有任意一种，直接赋值。若没有进入第二步

 2、查找accessInstanceVariablesDirectly是否允许访问成员变量，若为YES，则查找实例变量name，\_name，isName，\_isName，查到任意一个则进行赋值

 3、setter方法和实例变量都没有找到，系统会执行该对象的setValue:forUndefinekey:抛异常

当通过valueForKey取值时，其底层执行流程为：

 1、查找是否有getName，name，isName，_name的get方法，若找到则根据找到的属性值类型，返回对应结果。若没找到进入第二步

 2、检查InstanceVariablesDirectly是否为YES，查找\_name，_isName，name，isName，查到直接获取对应的值

 3、getter方法和实例变量都没找到，系统会执行valueForUndefinekey方法抛异常

# Runtime

C语言是静态类语言，编译阶段确定所有变量类型，OC是动态语言，编译并不知道变量类型，运行时才确定。runtime是一套比较底层的纯C语言API，编写OC代码中，程序运行过程中，其实最终都是转成了runtime的c语言代码

# 计时器

NSTimer、CADisplayLink、dispatch_source_t

# RunLoop

每个线程都有唯一一个与之对应的RunLoop对象

主线程RunLoop自动创建，子线程RunLoop需手动创建



CFRunLoopModeRef代表RunLoop的运行模式

一个 RunLoop 包含若干个 Mode，每个Mode又包含若干个Source/Timer/Observer

每次RunLoop启动时，只能指定其中一个 Mode，这个Mode被称作CurrentMode

如果需要切换Mode，只能退出Loop，再重新指定一个Mode进入

这样做主要是为了分隔开不同组的Source/Timer/Observer，让其互不影响



- NSDefaultRunLoopMode

  主线程下运行

- NSTrackingRunLoopMode

  跟踪用户交互事件

- UInitialzationRunLoopMode

  启动App时进入，启动完成后不再使用

- NSRunLoopCommonModes

  伪模式，不是一种真正的运行模式，是同步Source/Timer/Observer到多个Mode中的一种解决方案

- GSEventReceiveRunLoopMode

  接受系统内部事件

# **什么是内存对齐**

元素是按照定义顺序一个一个放到内存中去的，但并不是紧密排列的。从结构体存储的首地址开始，每个元素放置到内存中时，它都会认为内存是按照自己的大小（通常它为4或8）来划分的，因此元素放置的位置一定会在自己宽度的整数倍上开始，这就是所谓的内存对齐

# 进程间通信方式

`URL Scheme`，外链；

`Keychain`，本质sqllite，存储在/private/var/Keychains/keychain-2.db，保存敏感信息；

`UIPasteboard`，剪切板；

`UIDocumentInteractionController`，app之间共享文档、文档预览、打印、发邮件和复制；

`local socket`；

`AirDrop`，通过AirDrop实现不同设备的App之间文档和数据的分享；

`UIActivityViewController`，iOS SDK中封装好的类在App之间发送数据、分享数据和操作数据；

`App Groups`，同一团队开发的多个应用能直接数据共享

# 修饰符

第一阶段是MRC，需要程序员手动创建手动释放，比如上面例子，这一阶段属性的修饰词为assign、retain、copy。第二阶段是ARC，编译器自动进行内存管理，这一阶段属性的修饰词为weak、strong

**assign**

简单赋值，不更改引用计数，用于修饰基础类型的数据（NSInteger）和C语言类型数据(int,float,double,char,bool)

**retain**

会使引用计数加1，ARC下已经不再使用，用strong代替

**copy**

建立一个索引计数为1的对象，在赋值的时使用传入值的一份拷贝，适用于NSString和block

**weak**

不增加引用计数，也不持有对象

**strong**

会使引用计数加1

# C++内存管理

堆区、栈区、常量区、静态区、代码区

new 申请空间

delete 释放空间