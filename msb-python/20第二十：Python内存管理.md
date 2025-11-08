# 第二十节：Python内存管理

讲师：肖斌

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1663830361047/1062c851ee7c4b8ba21d35ada835ffdc.png)

## 一、Python对象引用计数

在Python中，每个对象都有存有指向该对象的引用总数，即引用计数(reference count)。容器类型放的都是引用

```

from sys import getrefcount
a = [1,2,3]
print(getrefcount(a))
b = a
print(getrefcount(a))
c = [a,a]
print(getrefcount(a))
print(getrefcount(b))
print(getrefcount(c))
 

a = [1,2,3]
b = a
c = [a,a]
print(getrefcount(a))
print(getrefcount(b))
print(getrefcount(c))
del c[0]
print(c)
print(getrefcount(a))
b = 2
print(getrefcount(a))

```

### 循环引用情况

x = []
y = []
x.append(y)
y.append(x)
对于上面相互引用的情况,删除x,y引用计数都减1，但是没有归0在内存中就不会释放，程序又不能访问这片空间，造成内存泄露。

## 二、垃圾回收（GC）

Python程语言的自动内存管理机制，当Python的某个对象的引用计数降为0时， 可以 被垃圾回收。

GC的作用：

• 找到内存中无用的垃圾资源

• 清除这些垃圾并把内存让出来给其他对象使用。

GC的效率：

* 垃圾回收时，Python不能进行其它的任务。频繁的垃圾回收将大大降低Python的工作效率。

GC的启动：

* 当Python运行时，会记录其中分配对象(object allocation)和取消分配对象(object deallocation)的 次数。当两者的差值高于某个阈值时，垃圾回收才会启动。

```
# import gc
# 常用函数：
gc.get_count()
# 获取当前自动执行垃圾回收的计数器，返回一个长度为3的列表

gc.get_threshold()
# 获取gc模块中自动执行垃圾回收的频率

gc.set_threshold(threshold0[,threshold1,threshold2])
# 设置自动执行垃圾回收的频率

gc.disable()
# python3默认开启gc机制，可以使用该方法手动关闭gc机制

gc.collect()
# 手动调用垃圾回收机制回收垃圾
 
>>> import gc
>>> print(gc.get_threshold())
(700, 10, 10)
>>>gc.collect()
2
>>>gc.collect()
0
```

### GC触发

1. 主动调用gc.collect()
2. GC达到阈值时自动触发
3. 程序退出时

### 自动GC机制一：引用计数

引用计数也是一种垃圾收集机制，而且也是一种最直观、最简单的垃圾收集技术。当Python的某个对象的引用计数降为0时，该对象就成为要被回收的垃圾了。不过如果出现循环引用的话，引用计数机制就不再起有效的作用了。

**问题：循环引用**

### 自动GC机制一：标记清除

标记-清除原理：当应用程序可用的内存空间被耗尽时，就会停止整个程序，然后进行两项工作，标记和清除

标记：通俗的讲就是，栈区相当于“根”，凡是通过根出发可以直接访问或者间接访问到的都是“有根之人”，有跟之人当活，无根之人当死；具体的讲，标记的过程其实就是遍历所有的GC Roots对象（栈区中的所有内容或者线程都可以称之为GC Roots对象），然后将其中可以直接或者间接访问到的对象都标记为可以存活的对象，其余的为非存活对象，应该清除掉

清除：清除所有没有被标记的非存活对象
**总结**

1. 标记：活动（有被引用）, 非活动（可被删除）
2. 清除：清除所有非活动的对象

当我们解除掉变量名l1和l2与两个列表的引用的时候，两个列表会被标记为非存活对象，等待清除，无需我们手动去清除

**问题：效率问题**

标记-清除需要遍历堆区中所有的对象，每次回收内存，都要遍历一遍，这个工作量是非常庞大的，因此即为耗时，于是引入了“分代回收”算法，分代回收采用了以空间换时间的策略。

### 自动GC机制三：分代(generation)回收

这一策略的基本假设是：存活时间越久的对象，越不可能在后面的程序中变成垃圾。
• Python将所有的对象分为0，1，2三代。
• 所有的 新建对象都是0代对象 。
• 当某一代对象经历过垃圾回收，依然存活，那么它就被归入下一代对象。
• 垃圾回收启动时，一定会扫描所有的0代对象。
• 如果0代经过一定次数垃圾回收，那么就启动对0代和1代的扫描清理。 • 当1代也经历了一定次数的垃圾回收后，那么会启动对0，1，2，即对所有对象进行扫描。

**总结：python采用的是引用计数机制为主，标记清除和隔代回收机制为辅的策略**

## 三、Python内存池

### Python整数内存池

整数缓冲区是在（-5~256）之间，系统已经初始化好，可以直接拿来用。而对于其他的大整数，系统则提 前申请了一块内存空间，等需要的时候在这上面创建大整数对象。

```python

>>> a = 1
>>> b = 1
>>> id(a)
140552326182816
>>> id(b)
140552326182816
>>> a =  257
>>> b  =  257
>>> id(a)
140552326841040
>>> id(b)
140552326841360
>>> b  =  256
>>> a =  256
>>> id(a)
140552326190976
>>> id(b)
140552326190976
>>> a =  -5
>>> b  = -5
>>> id(a)
140552326182624
>>> id(b)
140552326182624
>>> b  = -6
>>> a =  -6
>>> id(a)
140552326841200
>>> id(b)
140552326841040
>>>
```

### Python字符缓存（字符串驻留区 ）

为了检验两个引用指向同一个对象，我们可以用is关键字。is用于判断两个引用所指的对象是否相同。

当触发缓存机制时，只是创造了新的引用，而不是对象本身。

```python
>>>a = "xxx"
>>> b = "yyy"
>>> id(a)
140552327032424
>>> id(b)
140552327032480
>>> b = "xxx"
>>> id(b)
140552327032424
>>>>>> b = "xxx"
>>> id(b)
140552327032424
>>> a = "xxx "
>>> id(a)
140552327032536
>>> b = "xxx  "
>>> id(b)
140552327032424
>>> a = "xxx_"
>>> b = "xxx_"
>>> id(a)
140552327032480
>>> id(b)
140552327032480
>>>
```

is：比较的是两个对象的id值是否相等，也就是比较俩对象是否为同一个实例对象。是否指向同一个内存地址

== ： 比较的两个对象的内容/值是否相等，默认会调用对象的eq()方法

```python
>>> a = 777
>>> b = 777
>>> a is b
False
>>> a == b
True
>>>
```

## 四、Python的浅深拷贝问题

### 1、浅拷贝

把原对象第一层的内存地址不加区分(不区分可变类型还是不可变类型)完全copy一份给新对象。

```python

>>> list1=["狄仁杰",18,[1,2]]
>>> list2=list1.copy() # 浅拷贝
>>> print(id(list1))
1845462466816
>>> print(id(list2))
1845462466944
>>>
>>> print(id(list1[0]),id(list1[1]),id(list1[2]))
1845462085776 140730232539328 1845461126720
>>> print(id(list2[0]),id(list2[1]),id(list2[2]))
1845462085776 140730232539328 1845461126720
>>>
# 可见浅拷贝只拷贝第一层的内存地址，第一层的内存地址与旧列表指向相同的内存空间


```

![](https://img-blog.csdnimg.cn/20201116184912404.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2MDkwNjc1,size_16,color_FFFFFF,t_70#pic_center)

**修改操作对浅拷贝的影响:**

```python

>>> list1[0]="孙尚香" 
>>> list1[2][0]=333 # 修改嵌套列表第一个元素
>>> list1[2][1]="张良" # 修改嵌套列表第二个元素
>
>>> list1
['孙尚香', 18, [333, '张良']]
>>> list2
['狄仁杰', 18, [333, '张良']]
>>>


```

![](https://img-blog.csdnimg.cn/20201116185321544.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2MDkwNjc1,size_16,color_FFFFFF,t_70#pic_center)

### 2、深拷贝

就是完整拷贝：**copy**.**deepcopy**(list1)

![](https://img-blog.csdnimg.cn/20201116203404967.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzQ2MDkwNjc1,size_16,color_FFFFFF,t_70#pic_center)
