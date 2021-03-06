# 基础数据类型

Int、Double、Float、Bool、**String**

# 集合类型

Array、Set、Dictionary

# 属性

```swift
class SomeClass {
	var width = 1.0 // 存储属性
    var doubleWidth: Double { // 只读计算属性
        width * 2
    }
    var height = 1.0 { // 属性观察器
        willSet {
            // 默认拥有newValue
        }
        didSet {
            // 默认拥有OldValue
        }
    }
}
```

# 方法

值类型的属性不能在它的实例方法中被修改，但加了`mutating`前缀的方法可以

```swift
struct Point {
    var a = 0
    mutating func edit(x: Int) {
        a += x
    }
}
```

```swift
class SomeClass {
    static func method1() // 静态方法，不能被重写
    class func method2() // 静态方法，可以被重写
}
```

# 异步

```swift
class SomeClass {
	func download(url: String) async -> [String] {
        ...
    } 
    func method() {
		let arr = await download(url: "www.abc.com") // 会被挂起，直到download返回
    }
}
```

# UITableView委托

```swift
numberOfRowsInSection -> Int
cellForRowAt -> UITableViewCell
numberOfSection -> Int
didSelectRowAt
didDeselectRowAt
```

# UICollectionView委托

```swift
numberOfSections -> Int
numberOfItemsInSection -> Int
cellForItemAt -> UICollectionViewCell 
```

# 线程

**NSThread**

需要管理线程的生命周期和线程同步

```swift
// 1.定义执行内容并执行
Thread.detachNewThread {} 
// 2.实例方法创建线程
let thread = Thread(target: self, selector: #selector(action), object: nil)
子线程调用exit停止子线程
主线程调用cancel标记子线程可以停止
```

**NSOperation**

不需关心线程管理和数据同步

```swift
// 定义执行内容
var operation: NSOperation = NSBlockOperation {}
// 定义完成回调
opearation.completionBlock {}
var queue: NSOperationQueue = NSOperationQueue()
queue.addOperation(operation)
```

**Grand Central Dispath**

```swift
// 同步执行
DispatchQueue.main.sync {}
// 异步执行
DispatchQueue.main.async {}
```

```swift
主队列+同步任务-死锁
主队列+异步任务-依次执行
串行队列+同步任务-依次执行
串行队列+异步任务-开启一个新线程
并发队列+同步任务-依次执行
并发队列+异步任务-开启多个线程
```

# 锁

```swift
objc_sync_enter
objc_sync_exit
```

```swift
let condition = NSCondition()
condition.lock() // 加锁
condition.unlock() // 解锁
condition.wait() // 等待，释放锁
condition.signal() // 唤醒
```

![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/3/7/161ff105c75a5a12~tplv-t2oaga2asx-watermark.awebp)

# Swift与Objective-C

swift引入了objective-c中没有的一些高阶数据类型(tuples)

swift还引入了可选类型(optional)，可选项的意思有两种：一是变量是存在的，
    例如等于X，二是变量值根本不存在。Optional类似于Objective-C中指向nil的指针，但是适用于所有的数据类型，而非仅仅局限于类，Optional 相比于Objective-C中nil指针更加安全和简明，并且也是Swift诸多最强大功能的核心。

**混编**

oc调用swift：import “工程名-swift.h” @objc

swift调用oc：桥接文件

# present和push区别

```swift
self.present(vc, animated: true, comlpetion: nil)
self.dismiss(animated: true, completion: nil)
```

```swift
self.navgationController?.pushViewController(vc, animated: true)
self.navgationController?.popViewController(animated: true)
```

present只能逐级返回,push能返回上一级,也可及返回到根vc,其他vc

https://www.raywenderlich.com/1000705-model-view-controller-mvc-in-ios-a-modern-approach

# NSArray与NSSet

NSArray内存中存储地址连续；NSSet不连续

NSArray遍历查找；NSSet效率高，使用hash查找

NSArray通过下标访问；NSSet通过anyObject访问



# class、struct、enum区别

| 区别                                     | class    | struct | enum   |
| ---------------------------------------- | -------- | ------ | ------ |
| **类型**                                 | 引用类型 | 值类型 | 值类型 |
| **继承**                                 | 可以     | 不可以 | 不可以 |
| **静态方法/方法/静态变量/当作协议/拓展** | 可以     | 可以   | 可以   |
| **变量**                                 | 可以     | 可以   | 不可以 |

# defer使用场景

defer 语句块中的代码, 会在当前作用域结束前调用, 常用场景如异常退出后, 关闭数据库连接；有多个 defer, 那么后加入的先执行；嵌套defer先执行外层，后执行内层

# String与NSString的关系与区别

1.能够相互转换
2.String是值类型, NSString是引用类型.

# Optional

`try?`和`try!`这两个都用于处理可抛出异常的函数, 使用这两个关键字可以不用写 do catch.

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

# 存储方式

- preference

  使用`NSUserDefaults`存储在沙盒的`Library/Preferences`中

- plist

  xml方式储存在沙盒的`Documents`中

- NSKeyedArchiver(归档)

- SQLite3

  使用C的函数对数据库进行操作，可控性强，能够跨平台

- CoreData

  是对SQLite的封装

# 应用沙盒

`Document:`适合存储重要的数据， iTunes同步应用时会同步该文件下的内容,（比如游戏中的存档）
 `Library/Caches：`适合存储体积大，不需要备份的非重要数据，iTunes不会同步该文件
 `Library/Preferences:`通常保存应用的设置信息, iTunes会同步
 `tmp:`保存应用的临时文件，用完就删除，系统可能在应用没在运行时删除该目录下的文件，iTunes不会同步

# RunLoop

**保持程序持续运行**，程序一启动就会开一个主线程，主线程一开起来就会跑一个主线程对应的RunLoop,RunLoop保证主线程不会被销毁，也就保证了程序的持续运行

**处理App中的各种事件**（比如：触摸事件，定时器事件，Selector事件等）

**节省CPU资源，提高程序性能**，程序运行起来时，当什么操作都没有做的时候，RunLoop就告诉CPU，现在没有事情做，我要去休息，这时CPU就会将其资源释放出来去做其他的事情，当有事情做的时候RunLoop就会立马起来去做事情

# ARC（Auto Reference Count）

自动统计该对象被多少引用变量引用，当引用计数器计数变为0时，ARC就会回收这个对象

# weak和unowned

都是用于不让变量引用计数增加

在循环引用中，生命周期长的引用生命周期短的用weak，生命周期短的引用生命周期长的用unowned

## 触摸事件传递

只有继承了UIResponder的对象才能接受并处理事件（UIApplication、UIViewController、UIView）

**传递方向：从里到外(从父到子)**

- 判断主窗口能否接受触摸事件
- 判断触摸是否在自身范围内，若是则遍历子控件找出合适的，若不是则返回上一级

**导致传递中断三种情况**

不允许交互：userInteractionEnabled = false

隐藏：isHidden = true

透明度：alpha<0.01

## 消息机制

使用观察者模式

## WKWebView

性能、稳定性、功能方面提升，内存占用变少

```java
WKNavigationDelegate
didStartProvisionalNavigation // 页面开始加载
didCommitNavgation // 内容开始返回
didFinishNavigation // 页面加载完成
didFailProvisionalNavigation // 页面加载失败
didReceiveServerRedirectForProvisionalNavgation // 接收到服务器跳转请求
decidePolicyForNavigationAction // 发送请求之前决定是否跳转
```

## View的drawRect

drawRect中所调用的重绘功能是基于Quartz 2D实现的

调用drawRect的方法

- 调用sizeThatFits后
- 设置contentMode为UIViewContentModeRedraw，每次设置或更改frame时自动调用
- 调用setNeedsDisplay

## CAShapeLayer和drawRect

（1）两种自定义控件样式的方法各有优缺点，CAShapeLayer配合贝赛尔曲线使用时，绘图形状更灵活，而drawRect只是一个方法而已，在其中更适合绘制大量有规律的通用的图形；
 （2）CALayer的属性变化默认会有动画，drawRect绘图没有动画；
 （3）CALayer绘制图形是实时的，drawRect多次重绘需要手动调用setNeedsLayout；
 （4）性能方面，CAShapeLayer使用了硬件加速，绘制同一图形会比用Core Graphics快很多，CAShapeLayer属于CoreAnimation框架，动画渲染直接提交给手机GPU，不消耗内，而Core Graphics会消耗大量的CPU资源。