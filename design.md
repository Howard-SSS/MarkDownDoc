# 创建型模式

## 工厂方法模式

定义一个用于创建对象的接口，但是让子类决定将哪一个类实例化。工厂方法模式让一个类的实例化延迟到其子类

使用场景：

- 不需要创建细节

```java
// 抽象工厂
interface Factory {
    Product create();
}
// 具体工厂
class FactoryA implements Factory {
    Product create() { return ProductA(); }
}
// 具体工厂
class FactoryB implements Factory {
    Product create() { return PriductB(); }
}
// 抽象产品
interface Product {}
// 具体产品
class ProductA implements Product {}
class ProductB implements Product {}
```

## 抽象工厂模式

提供一个创建一系列相关或相互依赖对象的接口，而无须指定它们具体的类

使用场景：

- 系统中有多个产品族，每次只使用某一产品族
- 不需要要创建细节

```java
// 抽象工厂
interface Factory {
    Mouse createMouse();
    Keyboard createKeyboard();
}
// 具体工厂
class FactoryA implements Factory {
    Mouse createMouse() { return new LuoJiMouse(); }
    Keyboard createKeyboard() { return new LuoJiKeyboard(); }
}
// 具体工厂
class FactoryB implements Factory {
    Mouse createMouse() { return new LeiShenMouse(); }
    Keyboard createKeyboard() { return new LeiBoKeyboard(); }
}
// 抽象产品
interface Mouse {}
interface KeyBoard {}
// 具体产品
class LeiShenMouse implements Mouse {}
class LuoJiMouse implements Mouse {}
class LeiBoKeyboard implements Keyboard {}
class LuojiKeyboard implements Keyboard {}
```

## 建造者模式

将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示

使用场景：

- 生产的产品有复杂的内部结构，包含多个成员属性
- 相同的创建过程可以创建不同的产品

```java
// 车类（产品）
class Car {
	private int spead; // 速度
    private int seatNum; // 座椅数
    private int wheelNum; // 轮子数
    // setter getter
}
// 车建造厂（抽象建造者）
abstract class CarBuilder {
    protected Car car = new Car();
    public abstract void buildSpead();
    public abstract void buildSeatNum();
    public abstract void buildWheelNum();
    public Car createCar() { return car; }
}
// 法拉利建造厂（具体建造者）
class FerrariBuilder extends CarBuilder {
    public void buildSpead() { car.setSpead(100); }
    public void buildSeatNum() { car.setSeatNum(2); }
    public void buildWheelNum() { car.setWheelNum(4); }
}
// 三轮车建造厂（具体建造者）
class TricycleBuilder extends CarBuilder {
    public void buildSpead() { car.setSpead(15); }
    public void buildSeatNum() { car.setSeatNum(1); }
    public void buildWheelNum() { car.setWheelNum(3); }
}
// 指挥者
class Controller {
    public Car construct(CarBuilder builder) {
        builder.buildSpead();
        builder.buildSeatNum();
        builder.buildWheelNum()
		return builder.createCar();
    }
}
```

## 单例模式

确保一个类只有一个实例，并提供一个全局访问点来访问这个唯一实例

使用场景：

- 系统只需一个实例对象

```java
// 饿汉式单例
class EagerSingleton {
	private volatile static EagerSingleton instance = new EagerSingleton();
    private EagerSingleton() {}
    public static EagerSingleton getInstance() { return instance; }
}
// 懒汉式单例
class LazySingletion {
    private static LazySingletion instance = null;
    private LazySingletion() {}
    public synchronized static LazySingletion getInstance() { 
        if (instance == null) {
            singleton = new LazySingletion();
        }
        return instance; 
    }
}
```

## 原型模式

使用原型实例指定待创建对象的类型，并且通过复制这个原型来创建新的对象

使用场景：

- 创建对象成本高
- 保存对象的状态

```java
class Circle {
	public int width, height;
    public Circle() {}
    public Circle(Circle circle) {
        this.width = circle.width;
        this.height = circle.height;
    }
    public Circle clone() { return new Circle(this); }
    public boolean equals (Object object) {
        if (!(object instanceof Circle))
            return false;
        Circle shape = (Circle)object;
        return width == shape.width && heigth == shape.height;
    }
}
```

# 结构型设计模式

## 装饰者模式

动态地给一个对象增加一些额外的职责。就扩展功能而言，装饰模式提供了一种比使用子类更加灵活的替代方案

使用场景：

- 不影响其他对象的情况下动态、透明的方式给单个对象添加职责

```java
// 图片类（抽象构件）
abstract class Image {
    public void display();
}
// JPG图片类（具体构件类）
class JPGImage extends Image {
    public void display() {} // jpg解码展示
}
// 图片装饰者类（抽象装饰者）
class ImageDecorator extends Image {
    private Imagae image;
    public ImageDecorator(Image image) { this.image = image; }
    public void display() { image.display(); }
}
// 边框类（具体装饰者）
class Border extends ImageDecorator {
    public Border(Image image) { super(image); }
    public void display() { super.display();this.border(); }
    public void border() {} // 添加边框
}
```

## 外观模式

为子系统中的一组接口提供一个统一的入口。外观模式定义了一个高层接口，这个接口使得这一子系统更加容易使用。



## 适配器模式

让一个类的接口转换成客户希望的另一个接口，适配器模式让那些接口不兼容的类可以一起工作

使用场景：

- 接口不符合系统的需求，需要更改

**类适配器**

```java
// 飞机接口（目标接口类）
interface Place {
    void fly(); // 起飞
}
// 车类（适配者类）
class Car {
    public void speedUp() {} // 加速
}
// 飞机适配器类
class PlaceAdapter extends Car implements Place {
    public void fly() {
        super.speedUp();
        // 收起轮子之类的拓展
    }
}
```

**对象适配器**

```java
// 目标抽象类
abstract class Plane {
    public void skyMove() {} // 飞行
    public abstract void landMove(); // 陆地驾驶
}
// 适配者类
class Car {
    public void move() {} // 陆地驾驶 
}
// 适配器类
public class PlaneAdapter extends Plane {
    private Car car;
    public PlaneAdapter() { this.car = new Car();}
    public void landMove() {
        car.move();
    }
}
```

## 享元模式

运用共享技术有效地支持大量细粒度对象的复用

使用场景：

- 存在大量相同的对象

```java
// 单个字母工厂（享元工厂）
class SingleWordFactory {
    private HashMap<Character, SingleWord> map = new HashMap<>();
    public SingleWordFactory() {
        for (char i = 'a'; i <= 'z'; i++) {
			Word temp = new Word(i);
            map.put(i, temp);
        }
    }
    Flyweight getFlyweight(char c) {
        return map.get(c);
    }
}
// 单个字母类（抽象享元类）
abstract class SingleWord { public abstract void add(int row, int col); }
// 字母类（具体享元类）
class Word extends SingleWord {
    private char intrinsic;
    public ConcreteFlyweight(char c) { intrinsic = c; }
    public void add(int row, int col) {} // 添加到该位置
}

```

## 代理模式

给某一个对象提供一个代理或占位符，并由代理对象来控制对原对象的访问

使用场景：

- 当一个对象需要另一个对象指导

```java
// 目的地委托类（抽象主题角色）
abstract class DestinationDelegate {
    public abstract void go();
}
// 司机类（真实主题角色），不知道怎么去目的地
class Driver {
    public DestinationDelegate delegate;
    public void drive() {
        if (delegate != null)
            delegate.go();
    }
}
// 乘客类（代理主题角色），知道怎么去目的地
class Passenger implements DestinationDelegate {
    private Driver driver;
    public Passenger() {
        driver.delegate = this;
    }
    public void go() {} // 指明该怎么走
}
```

## 桥接模式

将抽象部分与它的实现部分解耦，使得两者都能够独立变化

使用场景：

- 存在两个独立变化的维度，不希望它们间使用多继承导致类的个数急剧增加

```java
// 系统类（抽象类）
abstract class System {
    protected VideoDecoding videoDecoding;
    public void setVideoDecoding(VideoDecoding videoDecoding) { this.videoDecoding = videoDecoding; }
    public abstract void play(String filePath); // 播放
}
// window类（扩充抽象类）
class Window implements System {
    void play(String filePath) {
        byte[] arr = videoDecoding.load(filePath);
        // 其他操作
    }
}
// linux（扩充抽象类）
class linux implements System {
    void play() {
        byte[] arr = videoDecoding.load(filePath);
        // 其他操作
    }
}
// 视频解码类（实现类接口）
interface VideoDecoding {
    byte[] load(String filePath); // 视频解码
}
// MPG解码类（具体实现类）
class MPG implements VideoDecoding {
    public byte[] load(String filePath) { } // T
}
// AVI解码类（具体实现类）
class AVI implements VideoDecoding {
    public byte[] load(String filePath) { }
}
```

## 组合模式

组合多个对象形成树形结构以表示具有部分-整体关系的层次结构。组合模式让客户端可以统一对待单个对象和组合对象

使用场景：

- 具有整体和部分层次结构中希望通过一种方式忽略整体与部分的差异，客户端可以一致地对待它们

**透明组合模式**

```java
// 抽象文件类（抽象构件）
abstract class AbstractFile {
    public boolean isAllowAdd() { return false; }
    public boolean isAllowRemove() { return false; }
    public boolean isAllowGetchild() { return false; }
    public abstract void add(AbstractFile object);
    public abstract void remove(AbstractFile object);
    public abstract AbstractFile getChild(int i);
    public abstract void visit();
}
// 文件类（叶子构件）
class File extends AbstractFile {
    public void add(AbstractFile object) {}
    public void remove(AbstractFile object) {}
    public AbstractFile getChild(int i) { return null; }
    public void visit() {} // 查看数理知识
}
// 文件夹类（容器构件）
class Folder extends AbstractFile {
    private ArrayList<AbstractFile> list = new ArrayList<>();
    public void add(AbstractFile object) { list.add(object); }
    public void remove(AbstractFile object) { list.remove(object); }
    public AbstractFile getChild(int i) { return list.get(i); }
    public void visit() {} // 罗列文件或文件夹
}
```

**安全组合模式**

```java
// 抽象文件类（抽象构件）
abstract class AbstractFile {
    public abstract void visit();
}
// 文件类（叶子构件）
class File extends AbstractFile {
    public void visit() {} // 查看数理知识
}
// 文件夹类（容器构件）
class Folder extends AbstractFile {
    private ArrayList<AbstractFile> list = new ArrayList<>();
    public void add(AbstractFile object) { list.add(object); }
    public void remove(AbstractFile object) { list.remove(object); }
    public AbstractFile getChild(int i) { return list.get(i); }
    public void visit() {} // 罗列文件或文件夹
}
```

# 行为型设计模式

## 观察者模式

定义对象之间的一种一对多的依赖关系，使得每当一个对象状态发生改变时其相关依赖对象皆得到通知并被自动更新

使用场景：

- 一个对象的改变将导致多个其他对象的也发生改变

```java
// 抽象哨兵类（目标）
abstract class AbstractSentry {
    private ArrayList<Soldier> list = new ArrayList<>();
    public void add(Soldier soldier) {
        if (!list.contains(soldier))
        	list.add(soldier);
    }
    public void remove(Soldier soldier) {
        list.remove(soldier);
    }
}
// 哨兵类（具体目标），情况有变负责通知其他部队
class Sentry extends AbstractSentry {
	public void notify() {
        for (Soldier temp : list)
            temp.action();
    }
}
// 士兵接口（观察者）
interface Soldier {
    void action();
}
// 步兵类（具体观察者）
class Infantry implements Soldier {
    public void action() {} // 准备冲锋
}
// 炮兵类（具体观察者）
class Artillery implements Soldier {
    public void action() {} // 占领高地
}
```

## 策略模式

定义一系列算法，将每一个算法封装起来，并让它们可以互相替换。策略模式让算法可以独立于使用它的客户而变化

使用场景

- 需要动态地在几种算法中选一种

```java
// 抽象策略类
abstract class Strategy { 
    public abstract void calculate(); 
}
// 深度遍历类（具体策略类）
class DFS implements Strategy { 
	void calculate() {} // 深度遍历
}
// 广度遍历类（具体策略类）
class BFS implements Strategy {
    void calculate() {} // 广度遍历
}
```

## 责任链模式

避免将一个请求的发送者与接收者耦合在一起，让多个对象都有机会处理请求。将接受请求的对象连接成一条链，并且沿着这条链传递请求，直到有一个对象能够处理它位置

使用场景：

- 多个对象处理同一请求

纯的责任链模式：处理者要么承担全部责任，要么推给下一位

不纯的责任链模式：处理者处理部分推给下一位，直到不被处理

```java
// 老师类（抽象处理者）
abstract class Teacher {
	public Teacher nextTeacher = null;
    abstract public boolean check(int num);
}
// 主任类（具体处理者）
class Director extends Teacher {
    public boolean check(int num) {
        if (num <= 1)
            return true; // 请假1天主任审批
        else
            return nextTeacher.check(num);
    }
}
// 院长类（具体处理者）
class Dean extends Teacher {
    public boolean check(int num) {
        if (num <= 2)
        	return true; // 请假2天院长审批
        else
            return nextTeacher.check(num);
    }
}
// 校长类（具体处理者）
class Headmaster extends Teacher {
    public boolean check(int num) { return true; } // 请假3天校长审批
}
```

## 状态模式

允许一个对象在其内部状态改变时改变它的行为。对象看起来似乎修改了它的类

使用场景：

- 对象的行为依赖状态

```java
// 状态类（抽象状态类）
abstract class State {
	void action();
}
// 类状态（具体状态类）
class Tired implements State {
    public void action() {} // 睡觉
}
// 兴奋状态（具体状态类）
class Funny implements State {
    public void action() {} // 笑
}
// 环境类
class Person {
    private State state = null;
    public void changeState(State state) { this.state = state; }
    public action() {
		if (state != null)
            state.action();
    }
}
```

## 命令模式

将一个请求封装为一个对象，从而可用不同的请求对客户进行参数化，对请求排队或者记录请求日志，以及支持可撤销的操作

使用场景：

- 命令类作为中间类，调用者和接收者相互间不必知道对方

```java
// 抽象命令类
abstract class Command {
    public abstract void execute();
}
// 打开文件类（接收者）
class OpenFile {
    public void open() {} // 打开文件
}
// 打开命令类（具体命令类）
class OpenCommand extends Command {
    private OpenFile openFile;
    public OpenCommand() { openFile = new OpenFile(); }
    public void execute() { openFile.open(); }
}
// 调用者
class Invoker {
    private Command command;
    public Invoker(Command command) { this.command = command; }
    public void call() { command.execute(); }
}
```

## 解释器模式

## 迭代器模式

提供一种方法顺序访问一个聚合对象中的各个元素，而又不用暴露该对象的内部表示

使用场景：

- 不希望暴露聚合对象内部
- 为聚合对象提供多种遍历方式

```java
// 抽象迭代器
interface IntIterator {
    int next();
    boolean hasNext();
    void remove();
}
// 抽象聚合类
abstract class AbstractIntList {
    protected ArrayList<Integer> list = new ArrayList<>();
    public void addInt(int val) { list.add(val); }
    public void removeInt(int val) { list.remove(val); }
    public abstract IntIterator iterator(); 
}
// 具体聚合类
class IntList extends AbstractIntList{
    public IntIterator iterator() { return new ListIterator(); }
    // 具体迭代器
    private class ListIterator implements IntIterator {
        private int index;
        public ListIterator() { index = 0; }
        public int next() { 
            if (index >= list.size())
                throw new NoSuchElementException();
            int temp = list.get(index++);
            return temp;
        }
        public boolean hasNext() { return index < list.size(); }
        public void remove() {
            if (list.size() > index - 1 && index - 1 >= 0)
                list.remove(index - 1);
        }
    }
}
```

## 中介模式

## 备忘录模式

