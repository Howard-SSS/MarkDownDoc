# 创建型模式

**工厂方法模式**

```java
interface Factory {
    Product create();
}
class FactoryA implements Factory {
    Product create() { return ProductA(); }
}
class FactoryB implements Factory {
    Product create() { return PriductB(); }
}
interface Product {}
class ProductA implements Product {}
class ProductB implements Product {}
```

**抽象工厂模式**

```java
interface Factory {
    Mouse createMouse();
    Keyboard createKeyboard();
}
class FactoryA implements Factory {
    Mouse createMouse() { return new LuoJiMouse(); }
    Keyboard createKeyboard() { return new LuoJiKeyboard(); }
}
class FactoryB implements Factory {
    Mouse createMouse() { return new LeiShenMouse(); }
    Keyboard createKeyboard() { return new LeiBoKeyboard(); }
}
interface Mouse {}
interface KeyBoard {}
class LeiShenMouse implements Mouse {}
class LuoJiMouse implements Mouse {}
class LeiBoKeyboard implements Keyboard {}
class LuojiKeyboard implements Keyboard {}
```

**建造者模式**

```java
class Car {
	private Engine engine;
    private int seats;
    public Car(Engine engine, int seats) {
        this.engine = engine;
        this.seats = seats;
    }
}
class Builder {
    private Engine engine;
    private int seats = 1;
    public void setEngine(Engine engine) { this.engine = engine; }
    public void setSeats(int seats) { this.seats = seats; }
    public Car getResult() { return new Car(engine, seats); }
}
```

**单例模式**

```java
final class Singleton {
	private static singleton = new Singletion();
    private Singleton() {}
    public static Singleton getSingleton() { return singleton; }
}
final class LazySingletion {
    private static singleton = null;
    private Singleton() {}
    public static Singleton getSingleton() { 
        if (singleton == null)
            singleton = new Singleton();
        return singleton; 
    }
}
```

**原型模式**

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

**装饰者模式**

```java
interface Decorator { Decorator add(Decorator decorator); }
class Fruit implements Decorator {
    Decorator inner;
    Decorator add(Decorator decorator) {
        inner.add()
    }
}
```

**适配器模式**

```java

```

**享元模式**

```java
class FlyweightFactory {
    private HashMap<Character, Flyweight> map = new HashMap<>();
    public FlyweightFactory() {
        for (char i = 'a'; i <= 'z'; i++) {
			ConcreteFlyweight temp = new ConcreteFlyweight(i);
            map.put(i, temp);
        }
    }
    Flyweight getFlyweight(char c) {
        return map.get(c);
    }
}
interface Flyweight { void point(int x, int y); }
class ConcreteFlyweight implements Flyweight {
    private char intrinsic;
    public ConcreteFlyweight(char c) { intrinsic = c; }
    public void point(int x, int y) {
        System.out.println(intrinsic + "in: [" + x + ", " + y + "]");
    }
}
```

**代理模式**

```java
interface Destination {
    void go();
}
class Driver {
    public Destination destination;
    public void drive() {
        if (destination != null)
            destination.go();
    }
}
class Passenger implements Destination {
    private Driver driver;
    public Passenger() {
        driver.proxy = this;
    }
    public void go() {
        // 先左转，再右转
    }
}
```

**桥接模式**

```java
interface Device {
    Word word = null;
    void compress();
}
interface Word {
    String save();
}
class TextWord implements Word {
    public String save() { System.out.println("文档格式保存"); }
}
class JsonWord implements Word {
    public String save() { System.out.println("Json格式保存"); }
}
class Window implements Device {
    void compress() {
        String temp = word.save();
        // zip压缩
    }
}
class Linus implements Device {
    void compress() {
        String temp = word.save();
        // gz压缩
    }
}
```

# 行为型设计模式

**观察者模式**

```java
class Nurse {
    private ArrayList<Doctor> list = new ArrayList<>();
    public void add(Doctor doctor) {
        if (!list.contains(doctor))
        	list.add(doctor);
    }
    public void remove(Doctor doctor) {
        list.remove(doctor);
    }
	void notify() {
        for (Doctor temp : list)
            temp.cure();
    }
}
interface Doctor {
    void cure();
}
class DoctorA implements Doctor {
    public void cure() {
        // 治疗内伤
    }
}
class DoctorB implements Doctor {
    public void cure() {
        // 治疗外伤
    }
}
```

**策略模式**

```java
interface Strategy { void calculate(); }
class StrategyA implements Strategy { 
	void calculate() {
        // 深度遍历
    }
}
class StrategyB implements Strategy {
    void calculate() {
        // 广度遍历
    }
}
```

**责任链模式**

```java
abstract class Teacher {
	public Teacher nextTeacher = null;
    abstract public void handleRequest(int num);
}
class TeacherA extends Teacher {
    public void handleRequest(int num) {
        if (num <= 1)
            //请假1天,TeacherA审批
        else
            handler.handleRequest(num);
    }
}
class TeacherB extends Handle {
    public void handleRequest(int num) {
        // TeacherB审批
    }
}
```

**状态模式**

```java
interface State {
	void action();
}
class Tired implements State {
    public void action() {
        // 睡觉
    }
}
class Funny implements State {
    public void action() {
        // 笑
    }
}
class Person {
    private State state = null;
    public void changeState(State state) { this.state = state; }
    public action() {
		if (state != null)
            state.action();
    }
}
```

