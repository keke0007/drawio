# 第十节：模块+异常+Debug

讲师：肖斌

## 一、Python模块

Python 模块(Module)，是一个 Python 文件，以 .py 结尾，模块能定义函数，类和变量，模块里也能包含可执行的代码。

### 1、导入模块

导入模块的5中方式

1. import 模块名
2. from 模块名 import 函数名
3. from 模块名 import  *
4. import 模块名 as 别名
5. from 模块名 import 函数名 as 别名

### 2、自定义模块

在Python中，每个Python文件都可以作为一个模块，模块的名字就是文件的名字。

**所以：自定义模块名必须要符合标识符命名规则。**

#### 2.1 定义模块

新建一个Python文件，命名为 `my_module1.py`，并定义 `test1`函数。

```python
def test1(a, b):
    print(a + b)
```

#### 2.2  测试模块

在实际开中，当一个开发人员编写完一个模块后，为了让模块能够在项目中达到想要的效果，这个开发人员会自行在py文件中添加一些测试信息.，例如，在 `my_module1.py`文件中添加测试代码。

```python
def test1(a, b):
    print(a + b)


test1(10, 10)
```

此时，无论是当前文件，还是其他已经导入了该模块的文件，在运行的时候都会自动执行 `testA`函数的调用。

解决办法如下：

```python
def test1(a, b):
    print(a + b)

# 只在当前文件中调用该函数才会执行，其他导入的文件内不符合该条件，则不执行test1函数调用
if __name__ == '__main__':
    test1(10, 10)
```

#### 2.3  `__all__`

如果一个模块文件中有 `__all__`变量，当使用 `from xxx import *`导入时，只能导入这个列表中的指定的元素。

`__all__` = ['test1']

## 二、Python的包

Python包体现了模块的结构化管理思想，包由模块文件构成，将众多具有相关功能的模块文件结构化组合形成包。

**注意：新建包后，包内部会自动创建** `__init__.py`文件，这个文件控制着包的导入行为。

**在** `__init__.py`文件中添加 `__all__ = [模块1, 模块2]`，控制允许导入的模块列表。

![](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fask.qcloudimg.com%2Fhttp-save%2Fdeveloper-news%2Feq2fm3c0hi.jpeg&refer=http%3A%2F%2Fask.qcloudimg.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1660829473&t=e225aa7b5c2e7b0f2780617fe921eb92)

## 三、安装第三方的Python库

安装命令：

pip install  库名字  # 最新版本

pip install 库名字==1.0.4 # 指定版本

**pip install  opencv-python   -i https://pypi.tuna.tsinghua.edu.cn/simple/**

**注意：-i  指定pip镜像源，这样下载速度快 ，默认采用Python官方的镜像源，由于是国外服务器，所以下载慢**

**其他国内镜像源：**

* 中国科学技术大学 : https://pypi.mirrors.ustc.edu.cn/simple/
* 豆瓣：https://pypi.douban.com/simple/
* 清华大学：https://pypi.tuna.tsinghua.edu.cn/simple/
* 阿里云 ： http://mirrors.aliyun.com/pypi/simple/

## 四、异常

执行时检测到的错误称为 ：*异常* ，异常不一定导致严重的后果；大多数异常不会被程序处理，而是显示下列错误信息：

```python
>>> 10 * (1/0)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ZeroDivisionError: division by zero
>>> 4 + spam*3
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
NameError: name 'spam' is not defined
>>> '2' + 2
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: can only concatenate str (not "int") to str
```

### 1、处理异常

捕获异常

```
try:
    可能发生错误的代码
except 异常类型1:
    如果捕获到该异常类型1执行的代码
except 异常类型2:
    如果捕获到该异常类型2执行的代码
......

```

**try 语句的工作原理如下：**

* 首先，执行 try 子句 （try 和 except 关键字之间的（多行）语句）。
* 如果没有触发异常，则跳过 except 子句，try 语句执行完毕。
* 如果在执行 try 子句时发生了异常，则跳过该子句中剩下的部分。 如果异常的类型与 except 关键字后指定的异常相匹配，则会执行 except 子句，然后跳到 try/except 代码块之后继续执行。
* 如果发生的异常与 except 子句 中指定的异常不匹配，则它会被传递到外部的 try 语句中；如果没有找到处理程序，则它是一个 未处理异常 且执行将终止。

```python
import sys
try:
    f = open('myfile.txt')
    s = f.readline()
    i = int(s.strip())
except OSError as err:
    print("OS error:", err)
except ValueError:
    print("Could not convert data to an integer.")
except Exception as err:
    print(f"Unexpected {err=}, {type(err)=}")
    raise
```

### 2、强制抛出异常

raise 语句支持强制触发指定的异常。比如：如果只想判断是否触发了异常，但并不打算处理该异常，则可以使用更简单的 raise 语句重新触发异常

```
>>> try:
...     raise NameError('HiThere')
... except NameError:
...     print('An exception flew by!')

```

### 3、Finally语句

finally表示的是无论是否异常都要执行的代码，例如关闭文件。一般都是做一些清理工作。

```python
try:
    f = open('test.txt', 'r')
except Exception as result:
    f = open('test.txt', 'w')
else:
    print('没有异常，真开心')
finally:
    f.close()
```

### 4、自定义异常类

程序可以通过创建新的异常类命名自己的异常。不论是以直接还是间接的方式，异常都应从 Exception 类派生。

异常类可以被定义成能做其他类所能做的任何事，但通常应当保持简单，它往往只提供一些属性，允许相应的异常处理程序提取有关错误的信息。

大多数异常命名都以 “Error” 结尾，类似标准异常的命名。

```python
# 自定义异常类，继承Exception
class TooShortInputError(Exception):
    def __init__(self, length, min_len):
        self.length = length
        self.min_len = min_len

    # 设置抛出异常的描述信息
    def __str__(self):
        return f'你输入的长度是{self.length}, 不能少于{self.min_len}个字符'


def main():
    try:
        con = input('请输入密码：')
        if len(con) < 3:
            raise TooShortInputError(len(con), 3)
    except Exception as result:
        print(result)
    else:
        print('密码已经输入完成')
```

## 五、with语句

`with`语句是Python中的一个上下文管理器，它可以帮助我们管理资源，确保在使用完毕后，资源被正确地释放。在Python中，`with`语句通常用于打开文件、数据库连接、处理事务等需要手动关闭的资源。


Python中使用 `with`语句的优势主要有以下几点：

1. 代码简洁：使用 `with`语句可以让代码更加简洁，不需要手动打开和关闭文件、数据库连接等资源，而是让Python自动处理这些细节。
2. 可读性强：使用 `with`语句可以让代码更加可读，因为它清晰地表达了代码块的开始和结束，使代码更加易于理解。
3. 安全性高：使用 `with`语句可以确保资源在使用完毕后被正确关闭，从而避免了资源泄露和其他潜在的问题。
4. 异常处理：使用 `with`语句可以更好地处理异常，如果在 `with`语句块中发生异常，Python会自动关闭资源，从而避免了资源泄露和其他潜在的问题。

```python
with open('file.txt', 'r') as f:
    # do something with f
```

## 六、Debug调试

### 1、打断点

一个断点标记了一个代码行，当Pycharm运行到该行代码时会将程序暂时挂起。断点会将对应的代码行标记为红色，取消断点的操作也很简单，在同样位置再次单击即可。**一定要debug运行才生效**

**注意：断点一般打在有报错嫌疑的代码前面**

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1693387416063/f5d6bb581f834fd997b0b64b6b905599.png)

### 2、Debug操作

![](https://img-blog.csdnimg.cn/20190527160454360.)接下来会Pycharm会执行以下操作：
（1）PyCharm开始运行，并在断点处暂停
（2）断点所在代码行变蓝，意味着Pycharm程序进程已经到达断点处，但尚未执行断点所标记的代码。
（3）Debug tool window窗口出现，显示当前重要调试信息，并允许用户对调试进程进行更改。


![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1693387416063/5e25796a1e394bc6b8abda76fdf927e1.png)

1.show execution point (F10) 显示当前所有断点
2.step over(F8) 单步调试。
若函数A内存在子函数a时，不会进入子函数a内执行单步调试，而是把子函数a当作一个整体，一步执行
3.step into(F7) 单步调试。
若函数A内存在子函数a时，会进入子函数a内执行单步调试。
4.step into my code(Alt + Shift +F7) 执行下一行但忽略libraries（导入库的语句）
5.force step into(Alt + Shift +F7) 执行下一行忽略lib和构造对象等
6.step out（Shift+F8）当目前执行在子函数a中时，选择该调试操作可以直接跳出子函数a，而不用继续执行子函数a中的剩余代码。并返回上一层函数。
7.run to cursor(Alt +F9) 直接跳到下一个断点
————————————————
版权声明：本文为CSDN博主「醒了的追梦人」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_33472146/article/details/90606359
