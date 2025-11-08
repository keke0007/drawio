# 第一节：Python入门（一）

**讲师：肖斌**

## 1、Python介绍

编程语言就是人和计算机进行交流的一种语言

```
	Python
	c / C++
	Java
	PHP
	C#
	go
	basic
	qbasic
	VB
	VC
```

Python 是一个高层次的结合了解释性、编译性、互动性和面向对象，同时也面向函数的脚本语言。

* **Python 是一种解释型语言：** 这意味着开发过程中没有了编译这个环节。类似于PHP和Perl语言。
* **Python 是交互式语言：** 这意味着，您可以在一个 Python 提示符 **>>>** 后直接执行代码。
* **Python 是面向对象语言:** 这意味着Python支持面向对象的风格或代码封装在对象的编程技术。
* **Python是面向函数的语言：** 这意味着Python支持面向函数的风格，更适合递归计算。
* **Python 是初学者的语言：** Python 对初级程序员而言，是一种伟大的语言，它支持广泛的应用程序开发

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1691560552043/cb7362a0a1684c5fbd78ba651547f8ff.png)

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1691560552043/7a6b559ee9e948fab34c3354f16cb634.png)

## 2、Python解释器

****作用：运行代码****

Python解释器的作用通俗理解，就是起到一个翻译的作用，让我们程序员所编写的代码计算机能读懂然后执行代码。比方说，现在有2个国家的人，一个A，一个B,现在A和B之间语言不通无法交流，这样怎么办，现在来个翻译官的会就能很好的交流了，简单的说现在Python解释器也就是起到这么一个翻译作用。其实计算机智能读懂0和1,0是关，1是开，咱们写的英文代码压根读不懂在计算机中运行不起来。所以我们在写代码之前必须给安装这个Python解释器。

**分类**

* CPython

官方的，基于C语言开发的解释器，是目前应用广泛的一个解释器，我们目前用的解释器就是这一款。

* IPython

基于CPython的一种交互式的解释器，用到相对较少

* 其他解释器
  Jython：运行在Java平台的解释器，直接把Python代码编译成Java字节码执行
  IronPython：运行在微软.Net平台上的Python计时器，可以直接把PYthon代码编译成.Net的字节码

**python解释权的版本**

* python2.x
* python3.x
  * python3.6
  * python3.7
  * Python3.8
  * Python3.9
  * python3.10

```
下载地址：https://www.python.org/downloads/release/python-3104/
```

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1652516723064/5717cf76056c4515b2523f06afc633c2.png)

**注意：我们可以用3.8，或者更新的3.9还有3.10等等**

## 3、Pycharm介绍

PyCharm是一种Python 的IDE（集成开发环境），带有一整套可以帮助用户在使用Python语言开发时：提高其效率的工具，内部集成的功能如下：

* **Project管理**
* **智能提示**
* **语法高亮**
* **代码跳转**
* **调试代码**
* **解释代码(解释器)**
* **框架和库**
* **......**

> **PythonCharm分为专业版（professional）和社区版（community）**
>
> 其中社区版是免费的。
>
> **下载地址：**[http://www.jetbrains.com/pycharm/download/#section=windows](http://www.jetbrains.com/pycharm/download/#section=windows)



![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1691560552043/84bcf9ee06fe41529772f2a5fdbae62b.png)

## 4、注释


<pre class="vditor-reset" placeholder="" contenteditable="true" spellcheck="false"><p data-block="0"><img src="https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1653294884069/d8cfcd2d965c459192e2f23920bfc68b.png" alt="image.png" class=""/></p><p data-block="0"><img src="https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1653294884069/db4c32d54f5a4f9c8fd9c3e4714780ea.png" alt="image.png"/></p></pre>

* 单行注释

* 多行注释
* 快捷键：ctrl + /
* 作用：对代码起到解释说明的作用,注释内容不是真的代码,并不执行

**设置代码模板**

```
# 马士兵教育
# @Time : ${DATE} ${TIME}
# @Author : 肖斌老师
# @Version : 
# @IDE : ${PRODUCT_NAME}
# @Project : ${PROJECT_NAME}

```

## 5、变量

我们的Python程序，处理的所有数据都必须在变量中。怎么去理解变量呢？官方的定义就是是变化的量叫变量，每一个变量都有变量名和变量值。从字面上看每个字都认识，但是好像不太好理解。我给大家举个例子 ：比如说我们要去五星级酒店找我的女朋友。可是酒店里面的客房非常多。如果你想快速找到你朋友，必须得知道你女朋友的房间号。根据房间号，立马可以知道房间的位置，打开门就能看见入住的客人。那这和变量有什么关系呢？其实啊，所有的变量都存在内存中。你把内存看成是一个大的五星酒店。每一间房就代表数据存放的内存地址范围。在这一个地址范围中存起来的就是我们的数据，对应客房中所住的客人。为了快速的查找或使用这个数据，通常我们把存储数据的地址范围和房间号一样，也定一个名称。这个名称就是变量。总结一下，房间就对应变量，房号对应变量名，入住的客人对应变量值。有了变量之后就可以快速的在茫茫（数海）的内存中。找到对应的数据。

**定义变量和使用：**

格式: 变量名 = 值
使用: 变量名


### 1  标识符

标识符命名规则是Python中定义各种名字的时候的统一规范，具体如下：

- 由数字、字母、下划线组成
- 不能数字开头
- 不能使用内置关键字
- 严格区分大小写

  ![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1653457010084/bbb5d981464d46b3b9bdf00f6a1575bd.png)

### 2 命名习惯

- 见名知义。
- 单词之间用下划线隔开：例如：`your_name`。

### 3、认识数据类型

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1653457010084/e68b9d4f8c2140a59d8b414d8fd7340e.png)
