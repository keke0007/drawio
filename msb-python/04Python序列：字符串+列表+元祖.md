# 第四节：Python的序列：字符串+列表+元祖

## 一、字符串

字符串是 Python 中最常用的数据类型。我们一般使用引号（单引号、双引号和三引号都可以）来创建字符串。字符串是由一个一个的字符组成的。**注意：字符串中可能会包含一种特殊字符——转义符**

```
var1 = 'Hello World!'
var2 = "Python \t Runoob"
```

### 1、Python中的转义符

python 用反斜杠 \  转义字符。

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1692182822017/51ba109fcc934bf79e1aa2138eb462a7.png)

### 2、下标

`“下标”`又叫 `“索引”`，就是编号。比如：学号，座位号，座位号的作用：按照编号快速找到对应的座位。同理，下标的作用即是通过下标快速找到对应的数据。

**注意：下标可以从0开始，反过来从-1开始**

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1692182822017/65c0b5f04910435f8cc6d5960c89b54a.png)

### 3、切片

切片是指对操作的对象截取其中一部分的操作。*字符串、列表、元组*都支持切片操作。

序列对象 [开始位置下标:结束位置下标:步长]； 口诀：包头不包尾。

```python
name = "abcdefg"

print(name[2:5:1])  # cde
print(name[2:5])  # cde
print(name[:5])  # abcde
print(name[1:])  # bcdefg
print(name[:])  # abcdefg
print(name[::2])  # aceg
print(name[:-1])  # abcdef, 负1表示倒数第一个数据
print(name[-4:-1])  # def
print(name[::-1])  # gfedcba
```

### 4、字符串中的函数

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1692182822017/e16b7c5923a64696b47dea00abc8bbee.png)

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1692182822017/1cc92060e3324755bf03e054e03886e4.png)

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1692182822017/90467d9fb5854dee98aacead8ba2ec65.png)

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1692182822017/2ae92a5032a848c892aef2a472bbef8c.png)

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1692182822017/bca938f4aebe4f33998f6a7812555423.png)

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1692182822017/3e5d26d449934c96878671dbf561ab7a.png)

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1692182822017/2a5bbf9c58e649408255aff2e41e6f6b.png)

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1692182822017/3b77f04536994e4eae7983da4f3a891a.png)

## 二、列表list

- 列表的格式

```python
[数据1, 数据2, 数据3]
```

- 常用操作方法
  - index()
  - len()
  - append()
  - insert()
  - pop()
  - remove()
  - reverse()
  - copy()

## 三、元祖（Tuple）

元组特点：元组使用小括号，且逗号隔开各个数据，数据可以是不同的数据类型，和列表一样都是有顺序的。

**注意：元组不支持修改，只支持查找**

**定义元组**

```
t1 = (10, 20, 30)

t2 = (10,)
```

**常用操作方法**

* **index()**
* **len()**
