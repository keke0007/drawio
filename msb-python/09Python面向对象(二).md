# 第九节：Python面向对象——继承和多继承

讲师：肖斌

## 一、继承

Python面向对象的继承指的是多个类之间的所属关系，即子类默认继承父类的所有属性和函数。

**在Python中，所有类默认继承object类，object类是顶级类或基类；**

### 1、单继承

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1693216064007/d447bb3ec2f64a2ebc105897009c66a0.png)

```python
class Fruit:
    color = '绿色'

    def harvest(self, color):
        print(f"水果是：{color}的！")
        print("水果已经收获...")
        print(f"水果原来是{Fruit.color}的!")


class Apple(Fruit):
    color = "红色"

    def __init__(self):
        print("我是苹果")


class Orange(Fruit):
    color = "橙色"

    def __init__(self):
        print("\n我是橘子")


apple = Apple()  # 实例化Apple()类
apple.harvest(apple.color)  # 在Apple()中调用harvest函数，并将Apple()的color变量传入
orange = Orange()
orange.harvest(orange.color)  # 在Orange()中调用harvest，并将Orange()的color变量传入

```

在编写类时，并不是每次都要从空白开始。当要编写的类和另一个已经存在的类之间存在一定的继承关系时，就可以通过继承来达到代码重用的目的，提高开发效率。

### 2、函数的重写

父类(被继承的类)的函数都会被子类(继承的新类)继承，当基类（父类）中的某个函数不完全适用于子类时，就需要在子类中重写父类的这个函数。 而且函数的名字必须一模一样。

```python

#父类
class Person:
  def __init__(mysillyobject, firstname, lastname):
    mysillyobject.name = firstname
    mysillyobject.family = lastname
 
  def myfunc(abc):
    print("Hello my name is " + abc.name)
 
#子类，继承
class Student(Person):
  def __init__(self, fname, year, lname):
    super().__init__(fname, lname) # 调用父类的属性赋值函数
    self.graduationyear = year  #子类自己的属性
 
  def welcome(self):
    #子类自己的函数
    print("Welcome", self.name, self.family, "to the class of", self.graduationyear)
 
x = Student("Elon", 2019, "Musk")
x.welcome(

```

Python 还有一个 `super()` 函数，通过使用 `super()` 函数，在重写父类函数时，让super().调用在父类中封装的函数.

如果在开发中，子类的函数中包含父类的函数，父类原本的函数是子类函数的一部分，就可以采用扩展的方式。

**扩展的方式步骤：**

1. 在子类中重写父类的函数
2. 在需要的位置使用 super().父类函数 来调用父类函数的执行
3. 代码其他的位置针对子类的需求，编写子类特有的代码实现

## 二、多继承

![image.png](https://fynotefile.oss-cn-zhangjiakou.aliyuncs.com/fynote/fyfile/482/1693216064007/703de0506b2d4345ae34cfb6b823b9c9.png)

在python中是可以多继承的，继承的先后顺序是有区别的，当我们调用函数的时候，如果第一个继承的找不到，才会去第二个中找，但是只要在第一个类中找到调用的那个函数，即使参数个数不匹配也不会调用第二个父类中的，此时会报错。

**MRO顺序：[Python](https://so.csdn.net/so/search?q=Python&spm=1001.2101.3001.7020)官方采用了一个算法将复杂结构上所有的类全部都映射到一个线性顺序上，而根据这个顺序就能够保证所有的类都会被构造一次。**

```python
类名字.__mro__
```

## 三、私有属性和函数

在Python中，可以为实例属性和函数设置私有权限，即设置某个实例属性或实例函数不继承给子类。甚至，**不能在类的外部调用和访问**

设置私有权限的方法：在属性名和函数名 前面 加上两个下划线 __。

如果也想要别人去访问和修改私有属性，**在Python中，一般定义函数名** `get_xx`用来获取私有属性，定义 `set_xx`用来修改私有属性值。
