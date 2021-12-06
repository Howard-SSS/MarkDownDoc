## 基本数据类型

byte/boolean/char/short/int/float/long/double

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
        Executors.newFixedThreadPool(5);
        Executors.newCachedThreadPool();
        Executors.newSingleThreadExecutor();
        Executors.newScheduledThreadPool(5);
        // https://blog.csdn.net/qq_40093255/article/details/116990431
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
        // 不同步、无序
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
