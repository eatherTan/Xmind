# Lamda的基本操作

> Stream 操作的 总览详解：  https://www.ibm.com/developerworks/cn/java/j-lo-java8streamapi/

#### Stream的特点

- 惰性化：惰性化的操作意思是，在执行Intermediate操作时，不立马执行，而是把这些函数放起来，当在执行Terminal操作时，一起执行Intermediate操作
- 支持并行化操作
- Stream流是单向的，不可往复的

#### 流的操作类型
Intermediate（中间操作）：中间操作时惰性（lazy）的，执行指定的操作后，返回一个新的Stream流。后面还可以继续接着操作流。

Terminal（终端操作）：当使用Terminal操作后，流就结束了，后面不能再继续操作流，这是流的最后一个操作。

#### Stream操作

**1.map 返回由指定函数应用于stream流的元素的结果 组成的流，即流中的每个元素都执行操作，然后返回执行操作后的元素再组成一个流，并且返回它。**

map 生成的是个 1:1 映射，每个输入元素，都按照规则转换成为另外一个元素
```
Stream<T> map(Function<? super T,? extends R> mapper)
```
举个例子：
```
//这里返回的的是imgs2的List过滤后的所有ImageUrl流，并且使用collect()
shopInfoDto.setLicenseImgs(imgs2.stream().map(p->p.getImageUrl()).
collect(Collectors.toList()));

//转换为大写
List<String> output = wordList.stream().
map(String::toUpperCase).
collect(Collectors.toList());

//平方数
List<Integer> nums = Arrays.asList(1, 2, 3, 4);
List<Integer> squareNums = nums.stream().
map(n -> n * n).
collect(Collectors.toList());

```
**2.filter：过滤出 满足Lamda表达式predicate元素的的流**

```
Stream<T> filter(Predicate<? super T> predicate) 
```
举个例子

```
List<BagOrderEntity> expArrivalList = waitDepositeList.stream()
.filter(s -> s.getExpArrivalTime().after(time))
.collect(Collectors.toList());
```

**3.peek：返回由该流的元素组成的流，另外在从生成的流中消耗元素时对每个元素执行提供的操作。**  

```
Stream<T> peek(Consumer<? super T> action)
```
举个例子

```
<!--该方法主要用于支持调试，您希望在流程中流过某个特定点时查看元素：-->
   Stream.of("one", "two", "three", "four") 
   .filter(e -> e.length() > 3) 
   .peek(e -> System.out.println("Filtered value: " + e)) 
   .map(String::toUpperCase) 
   .peek(e -> System.out.println("Mapped value: " + e)) .collect(Collectors.toList());  
```

**4.forEach：遍历元素。接收一个Lamda表达式，Stream流中每个元素，执行Lamda表达式方法中的操作。**

注意：forEach 不能修改自己包含的本地变量值，也不能用 break/return 之类的关键字提前结束循环。并且foreach是Terminal操作，执行到这个方法，流已经结束啦！=
```
void forEach(Consumer<? super T> action) 
```
举个例子：
```
roster.stream()
 .filter(p -> p.getGender() == Person.Sex.MALE)
 .forEach(p -> System.out.println(p.getName()));
```

peek: 可以到达遍历的效果，但是peek是属于Intermediate操作的，执行遍历操作以后，还会返回一个新的Stream流

```
Stream.of("one", "two", "three", "four")
 .filter(e -> e.length() > 3)
 .peek(e -> System.out.println("Filtered value: " + e))
 .map(String::toUpperCase)
 .peek(e -> System.out.println("Mapped value: " + e))
 .collect(Collectors.toList());
```
**5.sorted : 对 Stream 的排序通过 sorted 进行**

```
Stream<T> sorted()

//使用特定的比较器来排序
Stream<T> sorted(Comparator<? super T> comparator)
```
优点：相对于数组排序：它比数组的排序更强之处在于你可以首先对 Stream 进行各类 map、filter、limit、skip 甚至 distinct 来减少元素数量后，再排序，这能帮助程序明显缩短执行时间

举个例子

```
List<Person> persons = new ArrayList();
 for (int i = 1; i <= 5; i++) {
 Person person = new Person(i, "name" + i);
 persons.add(person);
 }
List<Person> personList2 = persons.stream().limit(2).sorted((p1, p2) -> p1.getName().compareTo(p2.getName())).collect(Collectors.toList());
System.out.println(personList2);
```
**6.流转换为其它数据结构**

```
// 1. Array
String[] strArray1 = stream.toArray(String[]::new);
// 2. Collection
List<String> list1 = stream.collect(Collectors.toList());
List<String> list2 = stream.collect(Collectors.toCollection(ArrayList::new));
Set set1 = stream.collect(Collectors.toSet());
Stack stack1 = stream.collect(Collectors.toCollection(Stack::new));
// 3. String
String str = stream.collect(Collectors.joining()).toString();
//转化为Map
Map map = stream.collect(Collectors.toMap(User::getId, Function.identity(), (key1, key2) -> key2));
```


#### 常见的lamda 取值的方式
```
// 获取初始化员工列表集合
List<Employee> employees = initList();

// 1. 获取所有的员工id集合
List<Integer> employeeList = employees.stream().map(Employee::getId).collect(toList());

// 2. 获取员工id与员工信息的Map集合
Map<Integer, Employee> idEmployeeMap = employees.stream().collect(Collectors.toMap(Employee::getId, Function.identity()));

// 3. 员工id与员工姓名一一对应
Map<Integer, String> idNameMap = employees.stream().collect(Collectors.toMap(Employee:: getId, Employee::getName));

// 4. 部门分组，一个部门id对应多个员工信息
Map<Integer, List<Employee>> departGroupMap = employees.stream().collect(Collectors.groupingBy(Employee::getDepartId));

// 5. 查询当前部门下员工的姓名集合
Map<Integer, List<String>> departNamesMap =employees.stream().collect(Collectors.groupingBy(Employee::getDepartId,Collectors.mapping(Employee::getName, Collectors.toList())));
```
## java8新特性--Stream将List转为Map汇总

**Stream将List转换为Map，使用Collectors.toMap方法进行转换**

背景：User类，类中分别有id，name,age三个属性。List集合,userList，存储User对象

1、指定key-value，value是对象中的某个属性值。
```
Map<Integer,String> userMap1 = userList.stream().collect(Collectors.toMap(User::getId,User::getName));
```

2、指定key-value，value是对象本身，User->User 是一个返回本身的lambda表达式
```
Map<Integer,User> userMap2 = userList.stream().collect(Collectors.toMap(User::getId,User->User));
```

3、指定key-value，value是对象本身，Function.identity()是简洁写法，也是返回对象本身
```
Map<Integer,User> userMap3 = userList.stream().collect(Collectors.toMap(User::getId, Function.identity()));
```

4、指定key-value，value是对象本身，Function.identity()是简洁写法，也是返回对象本身，key 冲突的解决办法，这里选择第二个key覆盖第一个key。
```
Map<Integer,User> userMap4 = userList.stream().collect(Collectors.toMap(User::getId, Function.identity(),(key1,key2)->key2));
```

#### Function.identity()的含义

Java 8允许在接口中加入具体方法。接口中的具体方法有两种，default方法和static方法，identity()就是Function接口的一个静态方法。

Function.identity()返回一个输出跟输入一样的Lambda表达式对象，等价于形如`t->t`形式的Lambda表达式

```
Stream<String> stream = Stream.of("I", "love", "you", "too");
Map<String, Integer> map = stream.collect(Collectors.toMap(Function.identity(), String::length));

== 等价于
Map<String, Integer> map = stream.collect(Collectors.toMap(t -> t, String::length));

```

 