## 一.面向对象初识

### 1.1 面向过程编程vs函数式编程

我们在没有学习函数的时候,写的代码都是面向过程式编程

```python
# 面向过程编程 测量对象的元素的个数。
s1 = 'fjdsklafsjda'
count = 0
for i in s1:
    count += 1


l1 = [1,2,3,4]
count = 0
for i in l1:
    count += 1
```

在我们学习函数后就是在面向函数编程

```python
def func(s):
    count = 0
    for i in s:
        count += 1
    return count
func('fdsafdsa')
func([1,2,3,4])
```

通过对比可知：函数编程较之面向过程编程最明显的两个特点：

**1，减少重复的代码。**

**2，增强代码的可读性。**

## 二.面向对象初识

### 2.1 函数式编程vs面向对象编程

函数式编程

```python
# 函数式编程

# auth 认证相关
def login():
    pass

def regisgter():
    pass

# account 账户相关
def func1():
    pass

def func2():
    pass


# 购物车相关
def shopping(username,money):
    pass
def check_paidgoods(username,money):
    pass
def check_unpaidgoods(username,money):
    pass
def save(username,money):
    pass
```

面向对象编程

```python
class LoginHandler:
    def login(self):
        pass

    def regisgter(self):
        pass

class Account:
    def func1(self):
        pass

    def func2(self):
        pass

class ShoppingCar:
    def shopping(username,money):
        pass
    def check_paidgoods(username,money):
        pass
    def check_unpaidgoods(username,money):
        pass
    def save(username,money):
        pass
```

通过对比可以看出面向对象第一个优点：

**面向对象编程：是一类相似功能函数的集合,使你的代码更清晰化，更合理化。**

说第二个优点之前，先看看什么是面向对象。

面向对象的程序设计的核心是对象（上帝式思维），要理解对象为何物，必须把自己当成上帝，上帝眼里世间存在的万物皆为对象，不存在的也可以创造出来。

问题来了,那什么是类?什么又是对象?

类：就是具有相同属性和功能的一类事物

对象：就是类的具体表现形式

具体一些：先解释解释什么是⻋? 有轱辘, 有⽅向盘, 有发动机, 会跑的是⻋. 好. 在解释⼀个. 什么是⼈. 有名字, 年龄, 爱好, 会唱歌跳舞思考的是⼈.那么广义上 车，人就是类：但是具体的我的车，你这个人这是一个对象。

猫，是一类，你们家养的 大橘。

狗，是一类，隔壁家养的那只二哈就是对象。

⾯向对象思维, 要⾃⼰建立对象. ⾃⼰建立场景. 你是就是⾯向对象世界中的上帝. 你想让⻋⼲嘛就⼲嘛. 你想让⼈⼲嘛⼈就能⼲嘛。

**再说第二个优点：面向对象，要拥有上帝的视角看问题，类其实就是一个公共模板（厂房），对象就从具体的模板实例化出来（慢慢体会）。**

### 2.2 类的结构

```python
class Human:
    """
    此类主要是构建人类
    """
    mind = '有思想'  # 第一部分：静态属性 属性 静态变量 静态字段
    dic = {}
    l1 = []
    def work(self): # 第二部分：方法 函数 动态属性
        print('人类会工作')
class 是关键字与def用法相同，定义一个类。
Human是此类的类名，类名使用驼峰(CamelCase)命名风格，首字母大写，私有类可用一个下划线开头。
类的结构从大方向来说就分为两部分：
静态变量。
动态方法。
```

## 三. 从类名的角度研究类

### 3.1 类名操作静态属性

第一种，查看类中的所有内容：类名.__dict__方式。

```python
class Human:
    """
    此类主要是构建人类
    """
    mind = '有思想'  # 第一部分：静态属性 属性 静态变量 静态字段
    dic = {}
    l1 = []
    def work(self): # 第二部分：方法 函数 动态属性
        # print(self)
        print('人类会工作')

print(Human.__dict__)
print(Human.__dict__['mind'])
Human.__dict__['mind'] = '无脑'
print(Human.__dict__)  # 错误
#通过这种方式只能查询，不能增删改.

# 第一种方式只用户查询全部内容（一般不用单独属性查询）.
```

 第二种：万能的点. 

```python
class Human:
    """
    此类主要是构建人类
    """
    mind = '有思想'  # 第一部分：静态属性 属性 静态变量 静态字段
    dic = {}
    l1 = []
    def work(self): # 第二部分：方法 函数 动态属性
        # print(self)
        print('人类会工作')
print(Human.mind)  # 查
Human.mind = '无脑'  # 改
print(Human.mind)
del Human.mind  # 删
Human.walk = '直立行走'
print(Human.walk)
# 通过万能的点 可以增删改查类中的单个属性
```

　　**对以上两种做一个总结：如果想查询类中的所有内容，通过 第一种__dict__方法，如果只是操作单个属性则用万能的点的方式。**

### 2.2 类名操作动态方法

　　前提：除了两个特殊方法：静态方法，类方法之外，一般不会通过类名操作一个类中的方法。

```Python
class Human:
    """
    此类主要是构建人类
    """
    mind = '有思想'  # 第一部分：静态属性 属性 静态变量 静态字段
    dic = {}
    l1 = []
    def work(self): # 第二部分：方法 函数 动态属性
        # print(self)
        print('人类会工作')
    def tools(self):
        print('人类会使用工具')

Human.work(111)
Human.tools(111)
下面可以做，但不用。
Human.__dict__['work'](111)
```

## 四. 从对象的角度研究类

### 4.1 什么是对象

**对象是从类中出来的，只要是类名加上（），这就是一个实例化过程，这个就会实例化一个对象。**

执行下列代码会发生什么事情？

```python
class Human:
    mind = '有思想'
    def __init__(self):
        print(666)
        print(self)  # <__main__.Human object at 0x00000191508AA828>

    def work(self): 
        print('人类会工作')

    def tools(self):
        print('人类会使用工具')
obj = Human() # 只要实例化对象，它会自动执行__init__方法
print(obj)  # <__main__.Human object at 0x00000191508AA828>
# 并且obj的地址与self的地址相同
```

其实实例化一个对象总共发生了三件事：

　　1，在内存中开辟了一个对象空间。

　　2，自动执行类中的__init__方法，并将这个对象空间（内存地址）传给了__init__方法的第一个位置参数self。

　　3，在__init__ 方法中通过self给对象空间添加属性。

示例：

```python
class Human:
    mind = '有思想'
    language = '使用语言'
    def __init__(self,name,sex,age,hobby):
        # self 和 obj 指向的是同一个内存地址同一个空间，下面就是通过self给这个对象空间封装四个属性。
        self.n = name
        self.s = sex
        self.a = age
        self.h = hobby

obj = Human('barry','男',18,'运动')
```

### 3.2 对象操作对象空间属性

对象查询对象中所有属性。 对象._dict_

```python
class Human:

    mind = '有思想'
    language = '实用语言'
    def __init__(self,name,sex,age,hobby):
        # self 和 obj 指向的是同一个内存地址同一个空间，下面就是通过self给这个对象空间封装四个属性。
        self.n = name
        self.s = sex
        self.a = age
        self.h = hobby

obj = Human('barry','男',18,'运动')
print(obj.__dict__)  # {'n': 'barry', 'h': '运动', 's': '男', 'a': 18}
```

对象操作对象中的单个属性。 万能的点 .

```python
class Human:

    mind = '有思想'
    language = '实用语言'
    def __init__(self,name,sex,age,hobby):
        # self 和 obj 指向的是同一个内存地址同一个空间，下面就是通过self给这个对象空间封装四个属性。
        self.n = name
        self.s = sex
        self.a = age
        self.h = hobby

obj = Human('barry','男',18,'运动')
obj.job = 'IT'  # 增
del obj.n  # 删
obj.s = '女' # 改
print(obj.s)  # 查
print(obj.__dict__)
```

### 4.3 对象查看类中的属性

```python
class Human:

    mind = '有思想'
    language = '实用语言'
    def __init__(self,name,sex,age,hobby):
        self.n = name
        self.s = sex
        self.a = age
        self.h = hobby

obj = Human('barry','男',18,'运动')
print(obj.mind)
print(obj.language)
obj.a = 666
print(obj.a)
```

### 4.4 对象操作类中的方法

```python
class Human:

    mind = '有思想'
    language = '实用语言'
    def __init__(self,name,sex,age,hobby):
        self.n = name
        self.s = sex
        self.a = age
        self.h = hobby

    def work(self):
        print(self)
        print('人类会工作')

    def tools(self):
        print('人类会使用工具')

obj = Human('barry','男',18,'运动')
obj.work()
obj.tools()
```

　　**类中的方法一般都是通过对象执行的（出去类方法，静态方法外），并且对象执行这些方法都会自动将对象空间传给方法中的第一个参数self.**

### 4.5 self 是什么?

self其实就是类中方法（函数）的第一个位置参数，只不过解释器会自动将调用这个函数的对象传给self。所以咱们把类中的方法的第一个参数约定俗成设置成self, 代表这个就是对象。

### 4.6 一个类可以实例化多个对象

```python
obj1= Human('小胖','男',20,'美女')
obj2= Human('相爷','男',18,'肥女')
print(obj1,obj2)
print(obj1.__dict__)
print(obj2.__dict__)
```