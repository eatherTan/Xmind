# Linux操作

Linux基础教材 ： http://ddrv.cn/docs/linuxtools/base/index.html

> 用一个日志文件来操作

## 1. grep文本搜索
常用参数：

### -e 匹配多个文本
展示 包含"Code Root"的行 与  包含"Avg"的行
```
cat gc-trcommander-172.29.219.175-1614310359.log | grep -e "Code Root" -e "Avg"
```


#### -o 只输出匹配的文本行
```
//只展示匹配 "Total time" 的内容
cat gc-trcommander-172.29.219.175-1614310359.log | grep -o "Total time"
```
#### -v 只输出不匹配的文本行
```
//展示没有匹配 "Total time" 的内容
cat gc-trcommander-172.29.219.175-1614310359.log | grep -v "Total time"
```
#### -c 统计单词出现的次数
```
cat gc-trcommander-172.29.219.175-1614310359.log | grep -c "Total time"
```
#### -n 打印匹配的行号
```
cat gc-trcommander-172.29.219.175-1614310359.log | grep -n "Total time"
```
**搜索结果**
```
// 18432 这个就是行号，搜索结果中新增了行号

./gc-trcommander-172.29.219.175-1614310359.log:18432:2021-02-27T17:08:01.180+0800: 106521.673: Total time for which application threads were stopped: 0.0002303 seconds, Stopping threads took: 0.0000240 seconds
./gc-trcommander-172.29.219.175-1614310359.log:18433:2021-02-27T17:09:01.213+0800: 106581.706: Total time for which application threads were stopped: 0.0010980 seconds, Stopping threads took: 0.0001498 seconds
./gc-trcommander-172.29.219.175-1614310359.log:18434:2021-02-27T17:09:01.213+0800: 106581.706: Total time for which application threads were stopped: 0.0002041 seconds, Stopping threads took: 0.0000224 seconds
./gc-trcommander-172.29.219.175-1614310359.log:18435:2021-02-27T17:10:01.188+0800: 106641.681: Total time for which application threads were stopped: 0.0012845 seconds, Stopping threads took: 0.0002001 seconds
```
#### -i 搜索时忽略大小写
搜索： 包含关键字（忽略大小写 application）的行，显示行号，并递归展示
```
cat gc-trcommander-172.29.219.175-1614310359.log | grep "Total time" | grep "Application"  -r -n -i
```
#### -l 只打印文件名
搜索： 包含关键字（忽略大小写 application）的文件名。（虽然后面加了-n显示行号，但是无效）
```
cat gc-trcommander-172.29.219.175-1614310359.log | grep "Total time" | grep "Application"  -r -n -i -l
```
```
//搜索结果 包含了关键字的文件名
stderr-trcommander-172.29.219.175-1614310359.log
vm-trcommander-172.29.219.175-1614310359.log
gc-trcommander-172.29.219.175-1614310359.log
csp/sentinel-record.log.2021-02-26.0
```

## 2. uniq 消除重复行

#### 消除重复行

```
//搜索包含  "Total time" ，且去重。 但是这样去重的话，只要一个不一样，都会认为是不同的，便不会去去重，所以可以可以添加参数，限制需要比较的内容-s 40 -w 10，如下
cat gc-trcommander-172.29.219.175-1614310359.log | grep "Total time" | uniq

//-s 40 -w 10 : -s 开始位置 -w 比较字符数
cat gc-trcommander-172.29.219.175-1614310359.log | grep "Total time" | uniq -s 40 -w 10

```
#### 找出重复行
```
sort unsort.txt | uniq -d
```

##  wc 统计行和字符的工具
```
wc -l file // 统计行数

wc -w file // 统计单词数

wc -c file // 统计字符数
```

## sort 排序
字段说明
- -n 按数字进行排序  
- -d 按字典序进行排序
- -r 逆序排序
- -k N 指定按第N列排序

```
sort -nrk 1 data.txt
sort -bd data // 忽略像空格之类的前导空白字符
```
## 查询网络服务和端口

#### 列出所有的端口

```
netstat -a
```

#### 查询7902端口现在运行什么程序:

```

```

#### 列出所有有监听的服务状态:

```
netstat -l
```

### 文件权限

```
chmod 777 file # 文件权限设为-rwxrwxrwx（用户/组/其他），可读可写可执行，口诀：读4写2执行1，全部都有777
```