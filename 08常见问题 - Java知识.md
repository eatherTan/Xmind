### Stream流的特性
- Stream 自己不会存储元素。
- Stream 不会改变源对象。相反，他们会返回一个持有结果的新Stream。
- Stream 操作是延迟执行的。这意味着他们会等到需要结果的时候才执行。

### == 和 equals()的区别
1. == 如果是引用类型对象是判断两个对象是否一样，比较的是内存地址；如果是基本数据类型比较的是值是否相等
2. equals() 如果重写了equals()方法，是比较两个值是否相等；如果没有重写equals()方法还是比较两个对象的内存地址是否相等

```
评： 这里我在思考的时候就忽略了分类来说， == 需要分基本数据类型和引用对象两种情况
```

### 深拷贝和浅拷贝，如何实现？
浅拷贝： 

1. 如果属性基本数据类型，就直接拷贝值。新对象中，基本数据类型的修改，不会引用之前的对象的值
2. 如果属性是引用类型，那就拷贝内存地址，两个对象的引用类型的成员变量的内存地址指向的是同一个内存地址！ 如果新对象修改了这个引用类型，也会影响到之前的对象

深拷贝：
1. 如果属性是基本数据类型，直接拷贝值。新对象对基本类型修改值，不会引用之前的对象的值
2. 如果属性是引用类型，在计算机中开辟一块新的内存地址用于存放复制的对象。


```
public class Subject {
private String name;
//省略getter,setter方法
}

注意： 实现Cloneable接口，重写clone方法
public class Student implements Cloneable{
     //引用类型
    private Subject subject;
    //基础数据类型
    private String name;
    private int age;
    //省略getter,setter方法
    
        /**
     *  重写clone()方法
     * @return
     */
    @Override
    public Object clone() {
        //浅拷贝
        try {
            // 直接调用父类的clone()方法
            return super.clone();
        } catch (CloneNotSupportedException e) {
            return null;
        }
    }
     @Override
    public String toString() {
        return "[Student: " + this.hashCode() + ",subject:" + subject + ",name:" + name + ",age:" + age + "]";
    }
}

public class ShallowCopy {
    public static void main(String[] args) {
        Subject subject = new Subject("yuwen");
        Student studentA = new Student();
        studentA.setSubject(subject);
        studentA.setName("Lynn");
        studentA.setAge(20);
        //使用
        Student studentB = (Student) studentA.clone();
        studentB.setName("Lily");
        studentB.setAge(18);
        Subject subjectB = studentB.getSubject();
        subjectB.setName("lishi");
        System.out.println("studentA:" + studentA.toString());
        System.out.println("studentB:" + studentB.toString());
    }
}
```


### ArrayList和LinkedList内部的实现大致是怎样的？他们之间的区别和各自适应的场景是什么？
ArrayList：

ArrayList使用数组实现的，初始化数组时有一个默认大小为10，插入元素时会判断要不然扩容，当数组增加的元素超过10个就会扩容，扩容后的数组是原来数组1.5倍+1；然后将原来的数组元素拷贝到新数组中，后面的元素都放到新数组中。原来的数组会被回收掉。

适用：ArrayList使用查找速度快的场景，查找的时间复杂度是O(1),插入和删除元素都很慢，需要移动元素，时间复杂度是O(n)

LinkedList：

LinkedList内部是使用双向链表是实现的，ListNode val， ListNode pre， ListNode next，当前节点值和前后节点的引用。

适用： LinkedList使用插入、删除操作多的场景，LinkedList插入删除操作快，无需移动元素，时间复杂度是O(1), 查找的时间复杂度是O(n)

### 请回答数组和链表的区别，以及优缺点，另外有没有什么办法能够结合两者的优点
哈希表可以结合数组和链表的优点。HashMap底层是数组，采用链式散列的结构。

HashMap 底层实现？
HashMap是采用数据+链表结合一起就是链式散列的结构

### 接口和抽象类的区别？
~~1. 接口不可以定义静态方法，可以定义静态变量；但是抽象可以都可以定义：静态方法、静态变量  这个错啦~~
2. 接口中的方法默认是public abstract
3. 接口的关键字是interface，抽象类的关键字是abstract
4. 接口实现类的关键字是implements，接口是 extends
5. 接口可以实现多继承，抽象类不支持多重继承
6. 接口中一定包含抽象方法，接口不一定，可以包含抽象方法和普通方法
7. 接口中只有final变量和静态变量，抽象类除了包含final变量和静态变量，还可以有普通变量

### 面向对象的特性
	封装（Encapsulation     [inˌkæpsjuˈleiʃən] ）
		类包含了数据与方法，将数据和方法放在一个类中就构成了封装
	继承（Inheritance          [ɪnˈherɪtəns]）
		继承就是在已存在的类上进行扩展，从而产生新的类
			子类不仅包含父类的属性和方法，还可以新增新的属性和方法
	多态（Polymorphism    [ˌpɒlɪˈmɔːfɪzm]）
		多态是一种运行时的行为，在运行时才知道该对象是什么类型的，在编译器并不知道。实现多态有三个必要条件：继承，重写，向上转型


​		
### 重写与重载的区别
1. 	重载发生在同一个类内部中的两个或多个方法
1. 	重写发生在子类与父类不同类之间，子类覆盖了父类的某一个方法
1. 	重载有相同的方法名，但是不同的参数
1. 	重写具有相同的返回类型和方法名参数
2. 	都是多态性的一种表现：重载是一个类中不同方法的多态性，重写是子类与父类之间多态性的表现

接口的限制
变量都被隐式的指定为static final
方法都被隐式的指定为public abstract
不能有具体方法

### 悲观锁、乐观锁

### 死锁
所谓死锁，是指多个进程在运行过程中因争夺资源而造成的一种僵局，当进程处于这种僵持状态时，若无外力作用，它们都将无法再向前推进。 因此我们举个例子来描述，如果此时有一个线程A，按照先锁a再获得锁b的的顺序获得锁，而在此同时又有另外一个线程B，按照先锁b再锁a的顺序获得锁。

### 进程与线程，线程有哪几种模式 要解决！！


面向对象的三大特性，并讲解一下？
HashMap 如果用对象作为key，可以吗 ？ 有什么要求
HashMap 如果用对象作为key，可以吗 ？ 有什么要求
还有HashMap 是线程安全的吗 ？ 如何保证线程安全
序列化和反序列化
异常讲解一下
CAS
Trasient关键字
文件上传动能的用例设计
测试流程，测试输出？
单例模式的写法？ 如何保证线程安全
静态内部类和内部类的区别

遍历Map的两种方式
```
//方法一
    HashMap map = new HashMap();
    map.put("a","aa");
    map.put("b","bb");
    map.put("c","cc");

    Set set = map.keySet();
    for (Iterator iterator = set.iterator(); iterator.hasNext();){
    //直接获取Map.Entry 。
        String key = (String) it.next();
        String value = (String) map.get(key);
        System.out.println(key);
        System.out.println(value);
    }

//方法二
    Map<String, String> map = new HashMap<String, String>();
    map.put("Java入门教程", "http://c.biancheng.net/java/");
    map.put("C语言入门教程", "http://c.biancheng.net/c/");

    for (Map.Entry<String, String> entry : map.entrySet()) {
        String mapKey = entry.getKey();
        String mapValue = entry.getValue();
        System.out.println(mapKey + "：" + mapValue);
    }
```

###  final有哪些用法?
1. 被final修饰的类不可以被继承
1. 被final修饰的方法不可以被重写
1. 被final修饰的变量不可以被改变.如果修饰引用,那么表示引用不可变,引用指向的内容可变.
1. 被final修饰的方法,JVM会尝试将其内联,以提高运行效率
1. 被final修饰的常量,在编译阶段会存入常量池中.

### 静态变量和实例变量的区别?
静态变量存储在方法区,属于类所有.实例变量存储在堆当中,其引用存在当前线程栈.需要注意的是从JDK1.8开始用于实现方法区的PermSpace被MetaSpace取代了.

### 线程和进程的区别？
线程是进程更小的单位；
一个进程可以有多个线程；
进程之前是独立的，但是线程却不一定；

### 线程安全
线程安全就是多线程访问时，采用了加锁机制，当一个线程访问该类的某个数据时，进行保护，其他线程不能进行访问直到该线程读取完，其他线程才可使用。不会出现数据不一致或者数据污染。

线程不安全就是不提供数据访问保护，有可能出现多个线程先后更改数据造成所得到的数据是脏数据

### 线程的声明周期：
1.new 新建状态
2.runnable 就绪状态
3.running 运行状态
4.blocked 堵塞状态

### 异常处理机制:
Java 的异常处理机制提供了一种**结构性和控制性**的方式来处理程序执行期间发生的事件。异常处理的机制如下：

1. 在方法中用 try catch 语句捕获并处理异常，catch 语句可以有多个，用来匹配多个异常。
2. 对于处理不了的异常或者要转型的异常，在方法的声明处通过 throws 语句拋出异常，即由上层的调用方法来处理。

### 获取某个类或某个对象所对应的Class对象的常用3种方式:

1使用Class对象的静态方法forName()
	
```
Class<?> classType  = Class.forName("java.lang.String");
```

2使用类的.class语法
	
```
Class<?> classType = String.class;
```
2使用对象的getClass() 方法
	
```
String s = "aa";
Class<?> classType = s.getClass()
```

### 创建对象的四种方式
1. 使用new关键字，new一个对象
2. 拷贝对象：深拷贝
3. 反射机制，获取class对象，class.newInstance();
4. 序列化


### Object类有什么方法？

```
equals()
hashCode()
toString()
wait()
notify()
notifyAll()
clone()
getClass() 
```

### String, StringBuilder, StringBuffer的区别、用法
1.String 是不可变的，它内部是由字符数组组成的，但是有final关键字修饰，所以是不可变的，其他的没有用final关键字修饰，所以是可变的
2.String适合用于操作不频繁的地方，因为它是不可变的，如果对象的内容修改了，那么原来的对象就会被废弃掉，对象指向新的内容，会产生大量的垃圾。
3.StringBuilder 适用于单线程操作大量数据
4.StringBuffer 适用于多线程操作大量数据，因为它的方法内部加了syncronized 关键字 

### 事务的特性ACID
- 原子性
- 一致性
- 隔离性
- 持久性

### 通俗讲解内存溢出和内存泄漏的区别
1. 内存溢出：在分配的java内存无法满足编写的程序时，会发生这种情况，比如用到的方法栈过深，大对象过多等

2. 内存泄漏：由于代码缺陷导致对象不断创建而垃圾回收无法及时回收对象，或者根本不回收，比如创建线程再不用后不关闭，频繁创建对象而不销毁等，长时间运行下来，再大的内存都会OutOfMemoryError

3. 重点说一下两者区别：内存溢出代码本身没有原则问题，最多算是代码品质不高，想要消除报错只能增加java分配的内存或者优化代码、而内存泄漏则是由于代码存在缺陷，如果不修改代码问题，即使分配再大的内存也会最终报错

