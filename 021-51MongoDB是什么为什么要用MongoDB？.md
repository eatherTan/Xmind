# `MongoDB`是什么,为什么要用`MongoDB`

`MongoDB`的中文手册：https://docs.mongoing.com/

在`MongoDB`中，文档集合(`Collection`)存在数据库中。

文档存储在文档集合(`Collection`)。文档集合(`Collection`)就是类似`MySQL`数据库的表。

`Collection`存储`mongoDB`文档，文档的格式可以很复杂，格式类似`Json`。`MongoDB`将数据记录存储为`BSON`文档，`BSON`是`Json`文档的二进制表示形式，它包含比`JSON`更多的数据类型。

#### 文档结构

字段的值可以是任何`BSON` 数据类型，包括其他文档，数组和文档数组。

> 字段名称`_id`保留用作主键；它的值在集合中必须是唯一的，不可变的，并且可以是数组以外的任何类型。如果插入的文档省略了该`_id`字段，则`MongoDB`驱动程序会自动为该`_id`字段生成一个`ObejctId`.

```
{
   field1: value1
}
如：
{
	"_id" : NumberLong(11466833),
    "pool_status" : 34,
    "apply_status" : 11
}
```

