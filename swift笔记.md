# present和push区别

present和dismiss对应,push和pop对应

present只能逐级返回,push能返回上一级,也可及返回到根vc,其他vc

https://www.raywenderlich.com/1000705-model-view-controller-mvc-in-ios-a-modern-approach

# ViewController生命周期

initWithCoder(类初始化)

init(对象初始化)

loadView

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

willMoveToSuperview

DidMoveToSuperview

willMoveToWindow

didMoveToWindow

layoutSubviews

drawRect

removeFromSuperview

dealloc

willRemoveSubview

# class、struct、enum区别

##### 相同点:

都可以定义属性和方法;
下标语法访问值;
初始化器;
支持扩展、协议

##### 区别:

class 是类, 引用类型,可以继承;
struct 是结构体,值类型, 结构体不可以继承;
struct在小数据模型传递和拷贝时比 class 要更安全，在多线程和网络请求时保证数据不被修改;

# defer使用场景

defer 语句块中的代码, 会在当前作用域结束前调用, 常用场景如异常退出后, 关闭数据库连接,如果有多个 defer, 那么后加入的先执行

# String与NSString的关系与区别

1.能够相互转换
2.String是值类型, NSString是引用类型.

# try?和try!是什么意思

这两个都用于处理可抛出异常的函数, 使用这两个关键字可以不用写 do catch.

##### 区别

try? 在用于处理可抛出异常函数时, 如果函数抛出异常, 则返回 nil, 否则返回函数返回值的**可选值**

而 try! 则在函数抛出异常的时候崩溃, 否则则返会函数**返回值**, 相当于(try? xxx)!

# 访问级别

|          | private | fileprivate  | internal | public                                 | open |
| -------- | ------- | ------------ | -------- | -------------------------------------- | ---- |
| 访问范围 | 类内    | .swift文件内 | 模块内   | 公开,但模块外不能复写方法/属性或被继承 | 公开 |

# 下面函数会打印什么

```csharp
var car = "Benz" 
let closure = {  [car] in 
  print("I drive \(car)")
} 
car = "Tesla" 
closure()
```

- 此时的car是局部变量, 因为 clousre(闭包) 已经申明将 car 复制进去了（[car]），不再与外面的 car有关，所以会打印出”I drive Benz”.
   如果修改一下呢?



```swift
var car = "Benz" 
let closure = {
  print("I drive \(car)")
} 
car = "Tesla" 
closure()
```

- 此时 closure 没有申明复制拷贝 car,所以clousre 用的还是全局的 car 变量，此时将会打印出 “I drive Tesla”

总结: 当声明闭包的时候，捕获列表会创建一份car的copy,所以被捕获到的值是不会改变的，即使你改变car的值。
 如果你去掉闭包中的捕获列表，编译器会使用引用代替copy,在这种情况下，当闭包被调用时，变量的值是可以改变的.

# Swift调用OC

创建Swift-Bridge-Objective-C.h

OC 用<u>#import "XXX.h"</u>导入

swift 用<u>import XXX</u>导入

# 存储方式

- NSUserDefaults
- plist
- SQLite3

# 应用沙盒

`Document:`适合存储重要的数据， iTunes同步应用时会同步该文件下的内容,（比如游戏中的存档）
 `Library/Caches：`适合存储体积大，不需要备份的非重要数据，iTunes不会同步该文件
 `Library/Preferences:`通常保存应用的设置信息, iTunes会同步
 `tmp:`保存应用的临时文件，用完就删除，系统可能在应用没在运行时删除该目录下的文件，iTunes不会同步

# RunLoop

**保持程序持续运行**，程序一启动就会开一个主线程，主线程一开起来就会跑一个主线程对应的RunLoop,RunLoop保证主线程不会被销毁，也就保证了程序的持续运行

**处理App中的各种事件**（比如：触摸事件，定时器事件，Selector事件等）

**节省CPU资源，提高程序性能**，程序运行起来时，当什么操作都没有做的时候，RunLoop就告诉CUP，现在没有事情做，我要去休息，这时CUP就会将其资源释放出来去做其他的事情，当有事情做的时候RunLoop就会立马起来去做事情