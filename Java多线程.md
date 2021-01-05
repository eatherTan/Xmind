# Java多线程

## 1. 对象及变量的并发访问

### 1. volatile 关键字

1.volatile的特性

1. 可见性。B线程可以看到A线程数据的修改
2. 原子性。
3. 禁止代码重排序

## 6. 单例模式与多线程

### 1.立即加载/饿汉模式

立即加载是指使用类的时候已经将对象创建完毕，常见的实现方法是直接用new实例化。从中文的语境看，立即加载有“着急” ”急迫“的意味，所以也称为”饿汉模式“。在立即加载 / 饿汉模式中，调用方法前，实例已经被工厂创建了。

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

延迟加载是指调用get() 方法时实例才被工厂创建，常见的方法在get()方法中进行new实例化。从中文的语境中，延迟加载有着“缓慢” “不急迫”的意味，所以也称为“懒汉模式”。在延迟加载/懒汉模式中，调用方法时才会被工厂创建。

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

在多线程环境中，延迟加载的代码是不正确的，不能保证单例。

#### 2.1延迟加载 / 饿汉模式的解决方案

1、声明synchronized关键字

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

2、尝试同步代码块

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

3、针对某些重要代码进行单独同步

```
public class Singleton {  
    private Singleton() {}  
    private static Singleton single=null;  
    //这种方法等同于synchronized public static Singleton getInstance()，效率一样很低
    public static Singleton getInstance() {  
    	try{
        	if (single == null) {  
        		synchronized (Singleton.class){
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



### 3.DCL Double Check Locking双重检查机制

使用volatile修饰变量single，使得变量在多个线程间达到可见性，另外也禁止了single = new Singleton();  代码进行重排序。

因为single = new Singleton();  代码在内部执行时分为三个步骤：

```
1. memory = allocate; //分配对象的内存空间
2. ctorInstance(memory); //初始化对象
3. single = memory; //设置instance指向刚分配的内存地址
```

编译器有可能将这三个步骤重排序成

```
1. memory = allocate; //分配对象的内存空间
2. single = memory; //设置instance指向刚分配的内存地址
3. ctorInstance(memory); //初始化对象
```

#### DCL机制

```
public class SingletonDCL {
    private volatile static SingletonDCL singleton;

    public SingletonDCL() {
    }

    public static SingletonDCL getInstance() {
        if (singleton== null) {
            synchronized (Singleton.class) {
                if (singleton== null) {
                    singleton= new SingletonDCL();
                }
            }
        }
        return singleton;
    }
}
```

