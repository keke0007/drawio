# Python正在表达式

## 一、re模块中三个函数

### 1、**match函数**

re.match 尝试从字符串的**起始位置**匹配一个模式，匹配成功则返回的是一个匹配对象（这个对象包含了我们匹配的信息），如果不是起始位置匹配成功的话，match()返回的是空。

**注意：match只能匹配到一个**

```python
s = 'python123python666python888'
#s = '1python123python666python888'

result = re.match('python', s)
print(result)  # <re.Match object; span=(0, 6), match='python'>
print(result.span())  # (0, 6)
print(result.group())  # python

```

* *通过span()提取匹配到的字符下标*
* *通过group()提取匹配到的内容*

### 2、search函数

re.search 扫描整个字符串,匹配成功则返回的是一个匹配对象（这个对象包含了我们匹配的信息）；当然,若是都找不到则返回None值。
**注意：search也只能匹配到一个，找到符合规则的就返回，不会一直往后找**

**总结：**

查找并返回一个匹配项的函数有3个：search、match、fullmatch，他们的区别分别是：

search： 查找任意位置的匹配项
match： 必须从字符串开头匹配
fullmatch： 整个字符串与正则完全匹配

### 3、findall函数

在字符串中找到正则表达式所匹配的所有子串，并返回一个列表，如果没有找到匹配的，则返回空列表

```python
s = '1python123python666python888'

result = re.findall('python', s)
print(result)  # ['python', 'python', 'python']

```

### 总结

#### 1.查找一个匹配项

查找并返回一个匹配项的函数有3个：search、match、fullmatch，他们的区别分别是：

search： 查找任意位置的匹配项
match： 必须从字符串开头匹配
fullmatch： 整个字符串与正则完全匹配

#### 2.查找多个匹配项

查找多项函数主要有：findall函数 与 finditer函数：

findall： 从字符串任意位置查找，返回一个列表
finditer：从字符串任意位置查找，返回一个迭代器

**正则表达式的应用方向：**

* 判断用户注册帐号是否满足格式
* 抓取页面中特定部分数据
* 判断用户提交的邮箱的格式是否正确 等等等等
  *那么我们就要了解元字符*

## 二、正则表达式的案例

### 1、案例一：匹配账号和密码

账号要求：

1.用户名只能包含数字 字母 下划线

2.不能以数字开头

3.⻓度在 6 到 16 位范围内

```python
import re

s = 'xzq_2234w'

result = re.match('^[a-z_][a-z\d_]{5,15}$', s)
print(result)  # <re.Match object; span=(0, 10), match='1587022xzq'>

```

因为账号是从头到尾的,只需要匹配一次就行,所以我们可以使用match方法

密码要求：

1.不能包含!@#¥%^&*这些特殊符号

2.必须以字母开头

3.⻓度在 6 到 12 位范围内

```python
s = 'xzq_2234w'
result = re.match(r'[a-z][^!@#¥%^&*]{5,11}', s)
print(result)
```

### 2、案例二：匹配QQ号码

要求： QQ号码的长度一般是5~10位， 全部都是数字，第一位不能是0

```python
s = '10086111222'
# 5 - 11 位
result = re.match(r'[1-9]\d{4,10}$', s)
print(result)  # <re.Match object; span=(0, 11), match='10086111222'>

```

### 3、案例三：检索所有的python源代码文件

要求： python文件名是一个整体，包含字母，数字和下划线_ 。

```python
s = '1.py 2.png ssx.csv qaq.txt xzq.py'
#文件名格式:  (数字字母_).py
result = re.findall(r'\w+\.py\b', s)
print(result)	# ['1.py', 'xzq.py']


```

### 4、案例四：验证邮箱地址

要求：邮箱地址验证，必须是网易邮箱或者是QQ邮箱。邮箱地址中必须有@，而且@符号前面至少5位，最多10位。

```python
# 验证输入的邮箱 163 126 qq 前面至少五位,至多11位
email = '738473800@qq.com'
result = re.match(r'\w{5,11}@(163|126|qq)\.(com|cn)$', email)
print(result)  # <re.Match object; span=(0, 16), match='738473800@qq.com'>


```

### 5、案例五：匹配座机号码

要求：座机号码必须是：区号-号码，而且区号包含3或者4位，号码一定是8位

```python
phone = '010-12345678'
result = re.match(r'(\d{3}|\d{4})-(\d{8})$', phone)
print(result)  # <re.Match object; span=(0, 12), match='010-12345678'>
print(result.group())  # 010-12345678
print(result.group(1))  # 010
print(result.group(2))  # 12345678

```

### 6、案例六：匹配html标签中的内容

要求：只提取合法标签中的内容，标签不要

```python
# 爬虫
s = '<html>hello</h1>'
s2 = '<html>hello</html>'
result = re.match(r'<([0-9A-z]+)>(.+)</\1>$', s)
print(result)  # None
result = re.match(r'<([0-9A-z]+)>(.+)</\1>$', s2)
print(result)  # <re.Match object; span=(0, 18), match='<html>hello</html>'>
print(result.group(1))  # html
print(result.group(2))  # hello


分组取名
# 取名字 (?P<名字>正则) (?P=名字)
s = '<html>hello</h1>'
s2 = '<html>hello</html>'
result = re.match(r'<(?P<name1>\w+)>(.+)</(?P=name1)>$', s)
print(result)  # None
result = re.match(r'<(?P<name1>\w+)>(?P<msg>.+)</(?P=name1)>$', s2)
print(result)  # <re.Match object; span=(0, 18), match='<html>hello</html>'>
print(result.group('name1'))  # html
print(result.group('msg'))  # hello


```

### 7、案例七：提取数字并计算

邀请：提取一个包含有数值的字符串中的数值 (数值包括正负数， 还包括整数和小数在内) 并求和。

例如:“-3.14good87nice19bye” =====> -3.14 + 87 + 19 = 102.86

```python
import re
from functools import reduce

s = '-3.14good87nice19bye'
nums = re.findall(r'-?\d+\.?\d*', s)
result = reduce(lambda x, item: x + float(item), nums, 0)
print(result)
```

### 8、案例八：匹配整数或者小数

要求： 匹配字符串是否，是整数或者小数，包含（正数或者负数，正数前面可以+，也可以没有）

```python
s = '-3.14'
result = re.fullmatch(r'[+-]?(0|[1-9]\d*)(\.\d+)?', s)

print(result)
```

[0-9a-z] ==(0-9a-z)
