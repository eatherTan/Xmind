# Selenium 定位页面元素方式

#### 1、通过元素的id属性来定位元素——id是唯一标识（每个id都是不一样的）

```
//id是唯一的，找到一个元素
WebElement element = driver.findElementById("fullTimeRegimentSettingRoot");
//有多个元素都为这个id
List<WebElement> elements = driver.findElementsById("fullTimeRegimentSettingRoot");
```

#### 2、通过tag名称来定位元素

```
//tagName在页面是唯一的，只找到一个元素
WebElement element = driver.findElementByTagName("input");
//tagName在页面 不是唯一的，有多个元素
List<WebElement> elements = driver.findElementsByTagName("input");
```

#### 3、通过类名来定位元素

```
WebElement element = driver.findElementByClassName("el-table__body-wrapper");
List<WebElement> elements = driver.findElementsByClassName("el-table__body-wrapper");
```

#### 4、通过xpath来定位元素

```
//找到一个元素
WebElement element = driver.findElementByXPath("//div[@id='fullTimeRegimentSettingRoot']//div[@class='el-table__body-wrapper is-scrolling-none']");
//找到多个元素
List<WebElement> elements = driver.findElementsByClassName("//div[@id='fullTimeRegimentSettingRoot']//div[@class='el-table__body-wrapper is-scrolling-none']");
```

xpath的路径写法有两种：相对定位和绝对定位

##### 相对定位：

**xpath基于xml的树状结构，提供在数据结构中找寻节点的能力**，代码比较长，且一旦有元素发生变化，可能就会失效，还有程序在运行的时候检索会比较慢，层层筛查。不推荐使用

```
WebElement element = driver.findElementByXPath("//*[@id="fullTimeRegimentSettingRoot"]/div[1]/div[1]/div[2]/button[1]")
```

相对定位：如下

```
WebElement element = driver.findElementByXPath("//div[@id='fullTimeRegimentSettingRoot']//div[@class='el-table__body-wrapper is-scrolling-none']");
```



#### 5、通过类名来定位元素

```
WebElement element = driver.findElementByClassName("input");
List<WebElement> elements = driver.findElementsByClassName("input");
```

#### 6、模糊查找contains

使用contains模糊定位，contains意为包含

```
模糊定位查找文本信息包含Potter的元素：
//title[contains(text(),"Potter")]
```

- @*表示所有属性
- not(@*)表示没有属性

#### 7、逻辑运算符

1.通过and运算符定位元素：category属性等于web && cover属性等于paperback

```
//book[@category="web" and @cover="paperback"]
```

2.通过or运算符定位元素

```
//book[@category="children" or @cover="paperback"]
```

3.通过取反not运算符定位元素

```
取book标签中position大于2的
//book[not(position()>2)] 
```

4、通过“>=”“<=”运算符定位元素

```
查找元素中是否存在price大于等于30的 存在返回Boolean true 不存在返回Boolean: false
//price>=30 
```

5、通过“！”运算符定位元素

```
//book[@category!='web' ]
```

#### 8、轴定位：

```
1.查找book[1]/title的父元素：//book[1]/title/parent::*
2.查找book标签下的所有子元素中标签为price的：//book/child::price 
3.following-sibling表示当前节点的后序所有兄弟节点元素
	查找title后面所有兄弟节点：//bookstore/book[1]/child::title/following-sibling::*
	指定查找title后面所有兄弟节点中名为author 的元素：/bookstore/book[1]/child::title/following-sibling::author
4.preceding-sibling::* 表示当前节点的前面所有兄弟节点元素
	查找price节点前面所有的兄弟元素：//bookstore/book[1]/child::price/preceding-sibling::* 
5.查找祖先节点包括自身：//book[1]/ancestor-or-self::*
6.查找子孙节点包括自身：//book[1]/descendant-or-self::*
7.查找当前节点的所有元素：//book[1]/preceding::* 查找当前节点下的所有元素
8.把book[2]以及book[2]节点之前的所有元素都找出来：//book[2]//preceding::* 
```

**轴总结：**

parent::* 表示当前节点的父节点元素
ancestor::* 表示当前节点的祖先节点元素
child::* 表示当前节点的子元素 /A/descendant::* 表示A的所有后代元素
self::* 表示当前节点的自身元素
ancestor-or-self::* 表示当前节点的及它的祖先节点元素
descendant-or-self::* 表示当前节点的及它们的后代元素
following-sibling::* 表示当前节点的后序所有兄弟节点元素
preceding-sibling::* 表示当前节点的前面所有兄弟节点元素
following::* 表示当前节点的后序所有元素
preceding::* 表示当前节点的所有元素

