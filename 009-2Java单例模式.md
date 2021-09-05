# [Java单例模式的写法]()

# 什么是单例模式？
单例模式是最简单的一种设计模式之一；

在程序内只有一个实例对象；单例模式不需要用户创建对象，用户可以直接使用`get()`方法获取对象。由该类内部自己负责创建对象，且保证只有单个对象被创建。

## Java单例模式的写法

```
java 单例模式有几种经典写法
1.饿汉模式
2.懒汉模式
3.双重检查模式
```

### 1.立即加载/饿汉模式

立即加载是指使用类的时候已经将对象创建完毕。 在立即加载 / 饿汉模式中，在调用方法前，实例已经被工厂创建了。

`private static final Singleton1 single = new Singleton1();` 可见该语句是用`static`关键字修饰的，因此在类加载阶段已经初始化了该对象，在调用`getInstance()`方法前，实例已经被工厂创建了。

且该方法能保证只有一个实例。没有现成安全问题，缺点是浪费内存空间，不管需不需要使用，都已经创建好了对象。

```
//饿汉式单例类.在类初始化时，已经自行实例化   
public class Singleton {  
    private Singleton() {}  
    private static final Singleton1 single = new Singleton1();  
    //静态工厂方法   
    public static Singleton1 getInstance() {  
        return single;  
    }  
}
```

### 2.延迟加载/懒汉模式

延迟加载是指调用`getInstance()` 方法时实例才被工厂创建，常见的方法在`getInstance()`方法中进行`new`实例化。从中文的语境中，延迟加载有着“缓慢” “不急迫”的意味，所以也称为“懒汉模式”。在延迟加载/懒汉模式中，调用方法时才会被工厂创建。

```
//懒汉式单例类.在第一次调用的时候实例化自己   
public class Singleton {  
    private Singleton() {}  
    private static Singleton single=null;  
    //静态工厂方法   
    public static Singleton getInstance() {  
         if (single == null) {    
             single = new Singleton();  
         }    
        return single;  
    }  
} 
```
> 延迟加载/懒汉模式的缺点
>
> 在多线程环境中，懒汉模式的代码有线程安全问题，不能保证单例。因此需要采取一定措施保证线程安全问题，如：同步锁。


#### 2.1延迟加载 / 饿汉模式的解决方案：DCL模式的演变过程

方案1：声明`synchronized`关键字 

```
public class Singleton {  
    private Singleton() {}  
    private static Singleton single=null;  
    //使用synchronized关键字，但是整个方法被上锁，效率低
    synchronized public static Singleton getInstance() {  
        try{
           if (single == null) {    
               single = new Singleton();  
           }    
           return single;  
        }catch(InterrupedException e)
            e.printStackTrace();
        }
    }  
} 
```
> 声明`synchronized`关键字 ，让代码变成同步的，解决了线程安全问题，但是整个方法被上锁，效率低。于是下面尝试采用`synchronized`锁部分代码，不锁住整个方法，尝试提高效率。
> 2、尝试同步代码块

```
public class Singleton {  
    private Singleton() {}  
    private static Singleton single=null;  
    //这种方法等同于synchronized public static Singleton getInstance()，效率一样很低
    public static Singleton getInstance() {  
        try{
           synchronized (Singleton.class){
               if (single == null) {    
                   single = new Singleton();  
               }    
               return single;  
           }
        }catch(InterrupedException e)
            e.printStackTrace();
        }
    }  
} 
```
> 这种方法等同于`synchronized public static Singleton getInstance()`，效率一样很低。于是再次减少锁住的代码，选择重要代码进行单独同步。如下：
> 3、针对某些重要代码进行单独同步

```
public class Singleton {  
    private Singleton() {}  
    private static Singleton single=null;  
    //这种方法等同于synchronized public static Singleton getInstance()，效率一样很低
    public static Singleton getInstance() {  
        try{
            if (single == null) {       //1.判断对象为空
                synchronized (Singleton.class){ //2.同步锁
                single = new Singleton();  
                } 
            }
            return single;  
        }catch(InterrupedException e)
            e.printStackTrace();
        }
    }  
} 
```

> `synchronized`锁住重要代码的方式，是既想达到线程安全的效果，又想效率提高。于是有了上面的代码。
>
> 但是仔细琢磨上面的代码会发现，以上代码有线程安全问题：
>
> 当有多个线程，如有线程A,B，这时两个线程都走到了1.判断对象为空的条件，两个线程都被锁住，此时只有一个线程能拿到锁，如：线程A拿到了锁，那么A线程往下走创建了对象，A线程释放锁。
>
> 然后B线程拿到锁，也会走创建对象的逻辑，这样就会造成多个实例对象的情况。因此需要在第一次对象判空之后，还需要再次判断一次对象为空才能创建对象。如下：

### 3.DCL Double Check Locking双重检查机制

#### DCL机制

```
public class SingletonDCL {
    private volatile static SingletonDCL singleton;

    public SingletonDCL() {
    }

    public static SingletonDCL getInstance() {
        if (singleton== null) {   //1.第1次check
            synchronized (Singleton.class) {
                if (singleton== null) {  //2.第2次check 为什么要double check？
                    singleton= new SingletonDCL();
                }
            }
        }
        return singleton;
    }
}
```

### 为什么要double check？- 第2次check

代码如上：进行了两次`singleton== null`对象不为空的判断，为什么要进行两次不为空的判断呢？

只进行一次`check` 的代码，如下

```
1   public static SingletonDCL getInstance() {
2        if (singleton== null) {   //1.第1次check
3            synchronized (Singleton.class) {
4                    singleton= new SingletonDCL();
5            }
6        }
7        return singleton;
8    }
```

1.如果进行一次`check`，多个线程同时进来，线程都被锁住在第3行代码上；

2.然后只有一个线程能拿到锁，如果线程A拿到了锁`new`了一个对象，然后释放锁；

3.线程B拿到锁，这时已经到了第一次check`if (singleton== null)`的后面了，又能创建一个对象；

这时就会有线程安全问题。所以需要在`syncronized`代码块中再check一次。这就是`Double Check`

### 单例双重锁为什么要用到volatile？

使用volatile的作用：
**使用volatile修饰变量singleton，使得变量在多个线程间达到可见性，另外也禁止了`singleton = new Singleton();` 代码重排序。**

因为`singleton = new Singleton();` 代码在内部执行时分为三个步骤：

```
1. memory = allocate; //分配对象的内存空间
2. ctorInstance(memory); //初始化对象
3. singleton = memory; //设置instance指向刚分配的内存地址
```

假如没有用到`volitile`，并发情况下会出现以下问题： 线程A执行到`singleton= new SingletonDCL();`的时候，编译器有可能将这三个步骤重排序成

```
1. memory = allocate; //分配对象的内存空间
2. single = memory; //设置instance指向刚分配的内存地址
3. ctorInstance(memory); //初始化对象
```


具体情况是：
线程1执行时`singleton= new SingletonDCL();`如果发生指令重排，执行顺序是132，执行到第3的时候，线程B刚好进来了，并且执行到注释2，这时候判断singleton不为空，直接使用一个未初始化的对象。所以使用`volatile`关键字来禁止指令重排序。

> 小贴士： 什么是指令重排序？
>
> 重排序是指编译器和处理器为优化程序性能而对指令序列重新排序的一种手段。

#### 指令重排序遵循的规则：

-   `Happens Before`
-   `As-if-serial`：不管怎么重排序（编译器和处理器为了提高并行度），（单线程）程序的执行结果不能被改变
-   CPU不会对任务操作进行重排序，编译器和处理器只会对**没有数据依赖**的指令进行重排序。

#### 什么是数据依赖

如果两个操作访问同一个变量，且这两个操作中有一个为写操作，此时这两个操作之间就存在数据依赖性。

| 名称   | 代码示例 | 说明                         |
| ------ | -------- | ---------------------------- |
| 写后读 | a=1;b=a  | 写一个变量之后，再读这个位置 |
| 写后写 | a=1;a=2  | 写一个变量之后，再写这个位置 |
| 读后写 | a=b;b=1  | 读一个变量之后，再写这个位置 |

上述三种情况，a与b存在着 **“数据依赖性”** ，同时大家也要注意。**这里所说的数据依赖性是指单个处理器执行的指令序列和单个线程中执行的操作。多处理器和不同线程之间是没有数据依赖性这种关系的。**