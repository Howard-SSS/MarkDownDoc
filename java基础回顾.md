## 基本数据类型

byte/boolean/char/short/int/float/long/double

## 封装/继承/多态

## ==和equals

==对于基本类型比较的是值，对于引用类型比较的是引用是否相同

String和Integer重写了equals方法，比较的是值

## StringBuffer和StringBuilder

它们都继承**AbstractStringBuilder**，**AbstractStringBuilder**使用字符数组保存字符串，是可变类

**StringBuffer**线程安全，**StringBuilder**非线程安全

## Math

| 静态函数名 | 描述     |
| ---------- | -------- |
| ceil       | 向上取整 |
| floor      | 向下取整 |
| max        | 最大     |
| min        | 最小     |
| round      | 四舍五入 |

## BigInteger/BigDecimal

| 函数名    | 描述 |
| --------- | ---- |
| add       | 加   |
| subtract  | 减   |
| multiply  | 乘   |
| divide    | 除   |
| compareTo | 比较 |

## 线程

```java
public class Main {
    public static void main(String args[]) {
        // Thread继承Runnable，实质重写Runnable run方法
        Thread thread1 = new Thread() {
            @Override
            public void run() {
                System.out.println("this is first method");
            }
        };
        // 重写Runnable run方法创建线程
        Thread thread2 = new Thread(() -> System.out.println("this is second method"));
        // FutureTask间接继承Runnable，实质重写Runnable run方法，并实现结束回调
        FutureTask<String> task = new FutureTask<String>(() -> "third method finish") {
            @Override
            public void run() {
                super.run();
                System.out.println("this is third method");
            }
        };
        Thread thread3 = new Thread(task);
        thread1.start();
        thread2.start();
        thread3.start();
        try {
            System.out.println(task.get());
        } catch(InterruptedException | ExecutionException e) { }
    }
}
```

## sleep和wait区别

sleep方法是Thread类的静态方法，不会释放资源

wait方法是Object类的方法，会释放资源，synchronize环境中调用

## 线程池

```java
public class Main {
    public static void main(String args[]) {
        /*
        1.在创建了线程池后,等待提交过来的任务请求.
		2.当调用execute()方法添加一个请求任务时,线程池就会做如下判断:
 		2.1 如果正在运行的线程数量小于corePoolSize,那么马上创建线程运行这个任务
  		2.2 如果正在运行的线程数量大于或等于corePoolSize,那么将这个任务放入队列
		2.3 如果这时候队列满了且正在运行的线程数量还小于maximumPoolSize,那么还是要创建非核心线程立刻运行这个任务
		2.4 如果对队列满了且正在运行的线程数量大于或等于maximumPoolSize,那么线程池会启动饱和拒绝策略来执行
		3.当一个线程完成任务时,它会从队列中取下一个任务来执行.
		4.当一个线程无事可做超过一定的时间(keepAliveTime)时,线程池会判断:
		4.1 如果当前运行的线程数大于corePoolSize,那么这个线程就会被停掉
		4.2 所以线程池的所有任务完成后它最终会收缩到corePoolSize的大小
        */
        // 实质创建ThreadPoolExecutor对象
        Executors.newFixedThreadPool(5);
        Executors.newCachedThreadPool();
        Executors.newSingleThreadExecutor();
        Executors.newScheduledThreadPool(5);
    }
}
```

| 方法                    | corePoolSize | maximumPoolSize   | keepAliveTime | workQueue           |
| ----------------------- | ------------ | ----------------- | ------------- | ------------------- |
| newFixedThreadPool      | nThreads     | nThreads          | 0             | LinkedBlockingQueue |
| newCachedThreadPool     | 0            | Integer.MAX_VALUE | 60s           | SynchronousQueue    |
| newSingleThreadExecutor | 1            | 1                 | 0             | LinkedBlockingQueue |
| newScheduledThreadPool  | corePoolSize | Integer.MAX_VALUE | 0             | DelayedWorkQueue    |

## 锁



## 反射

　　Java反射就是在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意方法和属性；并且能改变它的属性

```java
package com.company;
public class Main {
    public static void main(String args[]) {
        try {
            Object contact = new Contact();
            Class c1 = contact.getClass();
            Field field = c1.getDeclaredField("name");
            field.setAccessible(true);
            System.out.println(field.get(contact));
            Class c2 = Contact.class;
            field = c2.getDeclaredField("phone");
            field.setAccessible(true);
            System.out.println(field.get(contact));
            Class c3 = Class.forName("com.company.Contact");
            System.out.println(c3.getMethod("formatByPhone").invoke(contact));
        } catch (ClassNotFoundException | NoSuchFieldException | NoSuchMethodException | IllegalAccessException | InvocationTargetException e) {
            e.printStackTrace();
        }
    }
}
class Contact {
    private String name = "jane";
    private String phone = "17857745933";
    public String formatByPhone() {
        return "86" + phone;
    }
}
```

## 序列化

简单说就是为了保存在内存中的各种对象的状态（也就是实例变量，不是方法），并且可以把保存的对象状态再读出来

```java
public class Main {
    public static void main(String args[]) {
        Person person1 = new Person();
        try {
            ByteArrayOutputStream out = new ByteArrayOutputStream();
            ObjectOutputStream oos = new ObjectOutputStream(out);
            oos.writeObject(person1);
            byte[] buffer = out.toByteArray();
            InputStream in = new ByteArrayInputStream(buffer);
            ObjectInputStream ois = new ObjectInputStream(in);
            Person person2 = (Person)ois.readObject();
            person2.show();
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
class Person implements Serializable {
    private int money = 199;
    private String name = "jenny";
    public void show() {
        System.out.println("money: " + money + " name: " + name);
    }
}
```



## 浅克隆/深克隆

浅克隆就是对象是新的，基本数据类型使用新的，但引用类型是旧的

```java
public class Main {
    public static void main(String args[]) {
        Car car = new Car();
        Person person1 = new Person();
        person1.setCar(car);
        person1.show();// money: 199 car: { number: 1234 }
        Person person2 = (Person)person1.clone();
        person1.setMoney(200);
        car.setNumber("4321");
        person2.show();// money: 199 car: { number: 4321 }
        person1.show();// money: 200 car: { number: 4321 }
    }
}
class Person implements Cloneable {
    private int money = 199;
    private Car car = null;
    public void setMoney(int money) {
        this.money = money;
    }
    public void setCar(Car car) {
        this.car = car;
    }
    public Car getCar() {
        return car;
    }
    public void show() {
        System.out.println("money: " + money + " car: { " + car.show() + " }");
    }
    @Override
    public Object clone() {
        Person person = null;
        try {
            person = (Person)super.clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return person;
    }
}
class Car {
    private String number = "1234";
    public void setNumber(String number) {
        this.number = number;
    }
    public String show() {
        return "number: " + number;
    }
}
```

深克隆

```java
public class Main {
    public static void main(String args[]) {
        Car car = new Car();
        Person person1 = new Person();
        person1.setCar(car);
        person1.show();// money: 199 car: { number: 1234 }
        Person person2 = (Person)person1.clone();
        person1.setMoney(200);
        car.setNumber("4321");
        person2.show();// money: 199 car: { number: 1234 }
        person1.show();// money: 200 car: { number: 4321 }
    }
}
class Person implements Cloneable {
    private int money = 199;
    private Car car = null;
    public void setMoney(int money) {
        this.money = money;
    }
    public void setCar(Car car) {
        this.car = car;
    }
    public Car getCar() {
        return car;
    }
    public void show() {
        System.out.println("money: " + money + " car: { " + car.show() + " }");
    }
    @Override
    public Object clone() {
        Person person = null;
        try {
            person = (Person)super.clone();
            person.car = (Car)car.clone();
        } catch (CloneNotSupportedException e) {}
        return person;
    }
}
class Car implements Cloneable {
    private String number = "1234";
    public void setNumber(String number) {
        this.number = number;
    }
    public String show() {
        return "number: " + number;
    }
    @Override
    public Object clone() {
        Car car = null;
        try {
            car = (Car)super.clone();
        } catch (CloneNotSupportedException e) {}
        return car;
    }
}
```

[ API ](https://www.matools.com/api/java8)

## 排序

```java
public class Main {
	public static void main(String args[]) {
		// 排序
        Integer[] temp = {1,2,5,4,3};
        Arrays.sort(temp); // 1,2,3,4,5
        Arrays.sort(temp, new Comparator<Integer>() {
            @Override
            public int compare(Integer first, Integer second) {
                return second - first;
            }
        });// 5,4,3,2,1
        Stack stack = new Stack<Integer>();
        stack.push(7);
        stack.push(5);
        stack.push(9);
        Collections.sort(stack, new Comparator<Integer>() {
            @Override
            public int compare(Integer first, Integer second) {
                return first - second;
            }
        });// 5,7,9
    }
}
```

```java
public class Main {
    public static void main(String args[]) {
        int[] array = {1,3,2,5,4,7,6,9,8};
        SortImpl sort = new MergeSort();
        sort.sort(array);
        Arrays.stream(array).forEach(System.out::println);
    }
}
interface SortImpl {
    void sort(int[] array);
    default void swap(int[] array, int i, int j) {
        int temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }
}
// 冒泡排序
class BubbleSort implements SortImpl {
    public void sort(int[] array) {
        int length = array.length;
        for(int i = 1;i < length; i++) {
            boolean isFinish = true;
            for(int j = 0;j < length - i;j++) {
                if(array[j] > array[j + 1]) {
                    swap(array, j,j + 1);
                    isFinish = false;
                }
            }
            if(isFinish) {
                break ;
            }
        }
    }
}
// 插入排序
class InsertSort implements SortImpl {
    public void sort(int[] array) {
        int length = array.length;
        for(int i = 1; i < length; i++) {
            int temp = array[i];
            int j = i;
            while(j > 0 && temp < array[j - 1]) {
                array[j] = array[j - 1];
                j--;
            }
            if (j != i) {
                array[j] = temp;
            }
        }
    }
}
// 归并排序
class MergeSort implements SortImpl {
    public void sort(int[] array) {
        sort(array, 0, array.length -1);
    }
    public void sort(int[] array, int start, int end) {
        if(start >= end){
            return;
        }
        int middle = (start + end) / 2;
        sort(array, start, middle);
        sort(array, middle + 1, end);
        int left = start, right = middle + 1;
        int[] tempArr = new int[end - start + 1];
        int index = 0;
        while(left <= middle && right <= end) {
            tempArr[index++] = array[left] < array[right] ? array[left++] : array[right++];
        }
        while(left <= middle) {
            tempArr[index++] = array[left++];
        }
        while(right <= end) {
            tempArr[index++] = array[right++];
        }
        for(index = start; index <= end; index++) {
            array[index] = tempArr[index - start];
        }
    }
}
```

| 排序算法 | 平均时间复杂度 | 最好情况    | 最坏情况    | 空间复杂度 | 排序方式  | 稳定性 |
| -------- | -------------- | ----------- | ----------- | ---------- | --------- | ------ |
| 冒泡排序 | O(n^2^)        | O(n)        | O(n^2^)     | O(1)       | In-place  | 稳定   |
| 选择排序 | O(n^2^)        | O(n^2^)     | O(n^2^)     | O(1)       | In-place  | 不稳定 |
| 插入排序 | O(n^2^)        | O(n)        | O(n^2^)     | O(1)       | In-place  | 稳定   |
| 希尔排序 | O(nlogn)       | O(nlog^2^n) | O(nlog^2^n) | O(1)       | In-place  | 不稳定 |
| 归并排序 | O(nlogn)       | O(nlogn)    | O(nlogn)    | O(n)       | Out-place | 稳定   |
| 快速排序 | O(nlogn)       | O(nlogn)    | O(n^2^)     | O(logn)    | In-place  | 不稳定 |
| 堆排序   | O(nlogn)       | O(nlogn)    | O(nlogn)    | O(1)       | In-place  | 不稳定 |
| 计数排序 | O(n+k)         | O(n+k)      | O(n+k)      | O(k)       | Out-place | 稳定   |
| 桶排序   | O(n+k)         | O(n+k)      | O(n^2^)     | O(n+k)     | Out-place | 稳定   |
| 基数排序 | O(n*k)         | O(n*k)      | O(n*k)      | O(n+k)     | Out-place | 稳定   |

## 容器

```java
public class Main {
	public static void main(String args[]) {
        // 集合
        // 不同步、键无序
        HashSet set = new HashSet<Integer>();
        set.add("1");//true [1]
        set.contains("1");// true [1]
        set.remove("1");// true []
        set.clear();// []
        set.isEmpty();// true []
        
        // 不同步、键有序
        TreeSet set1 = new TreeSet<Integer>();
    }
}
```

- HashSet底层由HashMap实现，HashSet的值存放于HashMap的key上，HashMap的value统一为PRESENT

```java
public class Main {
	public static void main(String args[]) {
        // HashTable 线程安全
        // 线程不安全、无序
        HashMap map = new HashMap<Integer, Integer>();
        map.put(1,1);//null [(1,1)]
        map.get(1);//1 [(1,1)]
        map.remove(1);//1 []
        map.size();// 0
    }
}
```

HashMap是数组与链表的结合体，当我们往Hashmap中put元素时,首先根据key的hashcode重新计算hash值,根绝hash值得到这个元素在数组中的位置(下标),如果该数组在该位置上已经存放了其他元素,那么在这个位置上的元素将以链表的形式存放,新加入的放在链头,最先加入的放入链尾.如果数组中该位置没有元素,就直接将该元素放到数组的该位置上

## 线性表

```java
public class Main {
	public static void main(String args[]) {
        // 数组实现、线程不安全、容量不够会复制到新的存储空间
        ArrayList list1 = new ArrayList<Integer>();
        // 数组实现、线程安全、容量不够会复制到新的存储空间
        Vector list2 = new Vector<Integer>();
        // 链表实现、线程不安全
        LinkedList list3 = new LinkedList<Integer>();
    }
}
```

```java
public class Main {
	public static void main(String args[]) {
        // 栈、继承Vector
        Stack stack = new Stack<Integer>();
        stack.push(1);// [1]
        stack.push(2);// [1, 2]
        stack.peek();// 2 [1, 2]
        stack.pop();// 2 [1]
    }
}
```

## 同步容器

```java
List<String> list = Collections.synchronizedList(new ArrayList<>());

Map<Integer, String> map = Collections.synchronizedMap(new HashMap<>());

Set<String> set = Collections.synchronizedSet(new HashSet<>());
```

CopyOnWriteArrayList

写时在拷贝的数组上操作，完成后，拷贝数组替换旧数组，适用读多写少场景，读操作并不能马上看到写结果

ConrurrentHashMap

默认被细分为16个段，每一个段都是一个细粒度的HashMap，通过put(K key,V value)，会根据hashcode获得在哪一个段，对该段加锁，实现线程安全，同样size()会对每一个段加锁，等于对整个ConcurrentHashMap加锁，在求大小总和

CopyOnWriteArraySet

与CopyOnWriteArraySet类似

**队列先放着**

## 1.8

[github-java8讲解译文](https://github.com/weiwosuoai/java8_guide)

##### 接口方法实例化

```java
interface Area {
    default double size(double radius) {
        return radius * radius;
    }
}
```

##### Lambda表达式

```java
Integer[] temp = {1,2,5,4,3};
Arrays.sort(temp, (first, second) -> second - first);// 5,4,3,2,1
```

Lambda部分限制

```java
@FunctionalInterface
interface Calculate {
    int total(int[] arr);
}
public class Main {

    int count1 = 0;

    static int count2 = 0;

    void test() {
        int count3 = 0;
        int[] arr = {1,3,5,7};
        Calculate calculate = (array) -> {
            int ret = 0;
            for(int item : array) {
                ret += item;
                count1 += 1;// 允许对成员变量赋值
                count2 += 1;// 允许对静态变量赋值
                count3 += 1;// 不允许对局部变量赋值，隐式为final类型
            }
            return ret;
        };
    }
}
```

##### 流

```java
public class Main {
    public static void main(String args[]) {
        List<Integer> list = new ArrayList<>();
        // 只能对实现了 java.util.Collection 接口的类做流的操作
        for(int i = 0; i < 10; i++) {
            list.add(i);
        }
        list.stream()
                .filter((input) -> input % 2 == 0)// 过滤奇数
                .map((input) -> input * 2)// 偶数翻倍
                .forEach(System.out::println);// 0, 4, 8, 12, 16
    }
}
```

##### 注解

## java值传递

参数是基本类型，实际传递基本类型字面量的副本

参数是对象，实际传递的是原始对象的副本地址

## 

## BIO、NIO、AIO

BIO（同步阻塞）

```java
class Server {
    ServerSocket serverSocket = null;
    private int port = 10233;// 0 ~ 65535
    public Server() {
        try {
            this.serverSocket = new ServerSocket(port);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    public void beginAccept() {
        while(true) {
            try {
                Socket socket = serverSocket.accept();
                DataInputStream dis = new DataInputStream(new BufferedInputStream(socket.getInputStream()));
                DataOutputStream dos = new DataOutputStream(new BufferedOutputStream(socket.getOutputStream()));
                String name = dis.readUTF();
                dos.writeUTF(name + "以记录");
                dos.flush();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

```java
class Client {
    Socket socket = null;
    public void beginConnect() {
        try {
            socket = new Socket("127.0.0.1", 10223);
            DataInputStream dis = new DataInputStream(new BufferedInputStream(socket.getInputStream()));
            DataOutputStream dos = new DataOutputStream(new BufferedOutputStream(socket.getOutputStream()));
            dos.writeUTF(getName());
            dos.flush();
            System.out.println(dis.readUTF());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    private String getName() {
        int[] arr = new int[26];
        for(int i = 0; i < 26; i++) {
            arr[i] = 'A' + i;
        }
        String ret = "";
        Random random = new Random();
        for(int i = 0; i < 4; i++) {
            ret += (char)(arr[random.nextInt(26)]);
        }
        return ret;
    }
}
```



NIO（同步非阻塞）

AIO（异步非阻塞）
