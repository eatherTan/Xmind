# TestNG

### 优点

1. 比Junit功能更全面
2. Junit更适合隔离性强的单元测试
3. TestNG 更适合复杂的继承测试

### 使用方法

maven添加TestNG

### 用法

1. @Test：标记在要执行的测试方法上面
2. @BeforeMethod：在所有的测试方法之前都会运行的
3. @AfterMethod：在所有的测试方法运行之后都会运行
4. @BeforeClass：在类运行之前会执行的方法
5. @AfterClass：在类运行之后会执行的方法
6. @BeforeSuit：对于套件测试，在此套件中的所有测试运行之前运行。

> 注： 套件测试是什么东西？ - 套件测试是一起运行的多个测试类。

##### xml测试套件

结构

```
<suite>
	<test>
		<classes>
			<class></class>
		</classes>
	</test>
</suite>
```

案例

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd">
<suite name="test1">
    <test name="login">
        <classes>
            <class name="com.course.testng.suit.SuiteConfig"/>
            <class name="com.course.testng.suit.LoginTest"/>
        </classes>
    </test>
    <test name = "pay">
        <classes>
            <class name="com.course.testng.suit.SuiteConfig"/>
            <class name="com.course.testng.suit.PayTest"/>
        </classes>
    </test>
</suite>
```

7. @AfterSuit：对于套件测试，在此套件中的所有测试运行之后运行。

### 执行顺序

Before：Suit - Class - Mehtod 

After：Method - Class - Suit

### 忽略测试

@Ignore：对于标记了该注解的方法，忽略该方法，跳过测试

@Test(enable = false)：不执行

### 分组测试

#### 方法分组：

用法：在方法上面使用分组，@Test(groups = "server")

#### 类分组：

1. 在类上使用@Test("xxx")

2. 在xml文件中，使用group标签，执行run 指定分组的Class

```
    <test name="onlyRunStu">
        <groups>
            <run>
                <include name="stu"/>
            </run>
        </groups>
        <classes>
            <class name="com.course.testng.group.GroupOnClass1"/>
            <class name="com.course.testng.group.GroupOnClass2"/>
            <class name="com.course.testng.group.GroupOnClass3"/>
        </classes>
    </test>
```

### 异常测试

在什么时候用到异常测试

在我们期望结果为某一个异常的时候，比如：我们传入了一些不合法的参数，程序抛出了异常，也就是说我们的预期结果就是这个异常

```
@Test(expectedExceptions = RuntimeException.class)
预期结果是抛出异常，如果抛出了对应的异常，则测试执行通过；否则测试执行失败
```

### 依赖测试

当前测试方法method依赖于其他方法method1，当method1出现错误时，method就不执行，跳过执行。

### 参数化测试

1.使用@Parameters({"name","age"})，参数在xml文件中维护：name是参数名称，value是参数值

```
<parameter name="name" value="zhangsan"></parameter>
```

2.使用DataProvider()

@DataProvider(name ="data")
@Test(dataProvider = "data")

```
package com.course.paramter;

import org.testng.annotations.DataProvider;
import org.testng.annotations.Parameters;
import org.testng.annotations.Test;

public class DataProviderTest {
    @Test(dataProvider = "data")
    public void testDataProvider(String name, int age){
        System.out.println("name:" + name + " ," + "age:" + age);
    }
    @DataProvider(name = "data")
    public Object[][] providerData(){
        Object[][] objects = new Object[][]{
                {"张三",10},
                {"李四",20},
                {"王五",14}
        };
        return objects;
    }
}
```

3.还可以根据不同方法传递不同参数，使用反射，可以获取方法的名字

### 多线程测试

1.使用Annotation方式执行多线程

```
//在方法上可以指定线程池,
//invocationCount 线程数
//threadPoolSize 线程池
@Test(invocationCount = 10, threadPoolSize = 3)
```

2.使用xml方式执行多线程

```
parallel="methods" thread-count="2"
```

 xml文件配置这种方式不能指定线程池，只有方法上才可以指定线程池

1. tests级别：不同test tag下的用例可以在不同的线程下执行，相同的test tag 下的用例只能在相同的线程下执行
2. method级别：所有用例都可以在不同的线程下去执行，当线程数不够时，不同方法的线程可能相同
3. classed级别：TestNG将在同一个线程中运行同一个类中的所有方法，但是每个类都将在一个单独的线程中运行

### 测试报告：ExtentReport

#### 使用步骤

1.在maven中添加extentreport 的依赖

```
<dependencies>
​        <dependency>
​            <groupId>com.relevantcodes</groupId>
​            <artifactId>extentreports</artifactId>
​            <version>2.41.1</version>
​        </dependency>
​        <!-- https://mvnrepository.com/artifact/com.vimalselvam/testng-extentsreport -->
​        <dependency>
​            <groupId>com.vimalselvam</groupId>
​            <artifactId>testng-extentsreport</artifactId>
​            <version>1.3.1</version>
​        </dependency>
​        <dependency>
​            <groupId>com.aventstack</groupId>
​            <artifactId>extentreports</artifactId>
​            <version>3.0.6</version>
​        </dependency>

</dependencies>
```

2.新增xml文件

2.1在xml文件中指定要执行的方法，以前的老方法suite ....

2.2新增监听器

```
<!--使用监听器-->
<listeners>
    <listener class-name="com.vimalselvam.testng.listener.ExtentTestNgFormatter"/>
</listeners>
```

3.运行xml文件

如果成功，则会新增一个test-output文件夹，立面包含测试报告

