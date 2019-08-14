# Python 反射及双下方法

## 一.反射

反射的概念是由Smith在1982年首次提出的，主要是指程序可以访问、检测和修改它本身状态或行为的一种能力（自省）。这一概念的提出很快引发了计算机科学领域关于应用反射性的研究。它首先被程序语言的设计领域所采用,并在Lisp和面向对象方面取得了成绩。

**python面向对象中的反射：通过字符串的形式操作对象相关的属性。python中的一切事物都是对象（都可以使用反射）**

四个可以实现自省的函数

下列方法适用于类和对象（一切皆对象，类本身也是一个对象）

### 1.1 对象的反射

```python
class Foo:
    f = '类的静态变量'
    def __init__(self,name,age):
        self.name=name
        self.age=age

    def say_hi(self):
        print('hi,%s'%self.name)

obj=Foo('egon',73)

#检测是否含有某属性
print(hasattr(obj,'name'))
print(hasattr(obj,'say_hi'))

#获取属性
n=getattr(obj,'name')
print(n)
func=getattr(obj,'say_hi')
func()

print(getattr(obj,'aaaaaaaa','不存在啊')) #报错

#设置属性
setattr(obj,'sb',True)
setattr(obj,'show_name',lambda self:self.name+'sb')
print(obj.__dict__)
print(obj.show_name(obj))

#删除属性
delattr(obj,'age')
delattr(obj,'show_name')
delattr(obj,'show_name111')#不存在,则报错

print(obj.__dict__)

对实例化对象的示例
```

### 1.2 对类的反射

```python
class Foo(object):
 
    staticField = "old boy"
 
    def __init__(self):
        self.name = 'wupeiqi'
 
    def func(self):
        return 'func'
 
    @staticmethod
    def bar():
        return 'bar'
 
print getattr(Foo, 'staticField')
print getattr(Foo, 'func')
print getattr(Foo, 'bar')
```

### 1.3 当前模块的反射

```python
import sys


def s1():
    print 's1'


def s2():
    print 's2'


this_module = sys.modules[__name__]

hasattr(this_module, 's1')
getattr(this_module, 's2')
```

### 1.4 其他模块的反射

```python
#一个模块中的代码
def test():
    print('from the test')
"""
程序目录：
    module_test.py
    index.py
  
  
当前文件：
    index.py
"""
# 另一个模块中的代码
import module_test as obj

#obj.test()

print(hasattr(obj,'test'))

getattr(obj,'test')()
```

### 1.5 反射的应用

了解了反射的四个函数。那么反射到底有什么用呢？它的应用场景是什么呢？

现在让我们打开浏览器，访问一个网站，你单击登录就跳转到登录界面，你单击注册就跳转到注册界面，等等，其实你单击的其实是一个个的链接，每一个链接都会有一个函数或者方法来处理。

没学反射之前的解决方式

```python
class User:
    def login(self):
        print('欢迎来到登录页面')
    
    def register(self):
        print('欢迎来到注册页面')
    
    def save(self):
        print('欢迎来到存储页面')


while 1:
    choose = input('>>>').strip()
    if choose == 'login':
        obj = User()
        obj.login()
    
    elif choose == 'register':
        obj = User()
        obj.register()
        
    elif choose == 'save':
        obj = User()
        obj.save()
```

学了反射之后解决方式

```python
class User:
    def login(self):
        print('欢迎来到登录页面')
    
    def register(self):
        print('欢迎来到注册页面')
    
    def save(self):
        print('欢迎来到存储页面')

user = User()
while 1:
    choose = input('>>>').strip()
    if hasattr(user,choose):
        func = getattr(user,choose)
        func()
    else:
        print('输入错误。。。。')
```

这样就可以明确的感觉到反射的好处

## 二. 双下方法

定义：双下方法是特殊方法，他是解释器提供的 由爽下划线加方法名加双下划线 __方法名__的具有特殊意义的方法,双下方法主要是python源码程序员使用的，我们在开发中尽量不要使用双下方法，但是深入研究双下方法，更有益于我们阅读源码。

调用：不同的双下方法有不同的触发方式，就好比盗墓时触发的机关一样，不知不觉就触发了双下方法，例如：__init__

### 3.1 \__len__

```python
class B:
    def __len__(self):
        print(666)

b = B()
len(b) # len 一个对象就会触发 __len__方法。

class A:
    def __init__(self):
        self.a = 1
        self.b = 2

    def __len__(self):
        return len(self.__dict__)
a = A()
print(len(a))
```

### 3.2 \__hash__

```python
class A:
    def __init__(self):
        self.a = 1
        self.b = 2

    def __hash__(self):
        return hash(str(self.a)+str(self.b))
a = A()
print(hash(a))
```

### 3.3 \__str__

如果一个类中定义了__str__方法，那么在打印 对象 时，默认输出该方法的返回值。

```python
class A:
    def __init__(self):
        pass
    def __str__(self):
        return '宝元'
a = A()
print(a)
print('%s' % a)
```

### 3.4 \__repr__

如果一个类中定义了__repr__方法，那么在repr(对象) 时，默认输出该方法的返回值。

```python
class A:
    def __init__(self):
        pass
    def __repr__(self):
        return '宝元'
a = A()
print(repr(a))
print('%r'%a)
```

### 3.5 \__call__

对象后面加括号，触发执行。

注：构造方法__new__的执行是由创建对象触发的，即：对象 = 类名() ；而对于 __call__ 方法的执行是由对象后加括号触发的，即：对象() 或者 类()()

```python
class Foo:

    def __init__(self):
        pass
    
    def __call__(self, *args, **kwargs):

        print('__call__')


obj = Foo() # 执行 __init__
obj()       # 执行 __call__
```

### 3.6 \__eq__

```python
class A:
    def __init__(self):
        self.a = 1
        self.b = 2

    def __eq__(self,obj):
        if  self.a == obj.a and self.b == obj.b:
            return True
a = A()
b = A()
print(a == b)
```

### 3.7 \__del__

析构方法，当对象在内存中被释放时，自动触发执行。

注：此方法一般无须定义，因为Python是一门高级语言，程序员在使用时无需关心内存的分配和释放，因为此工作都是交给Python解释器来执行，所以，析构函数的调用是由解释器在进行垃圾回收时自动触发执行的。

### 3.8 \__new__

```python
class A:
    def __init__(self):
        self.x = 1
        print('in init function')
    def __new__(cls, *args, **kwargs):
        print('in new function')
        return object.__new__(A, *args, **kwargs)

a = A()
print(a.x)
```

使用__new__实现单例模式

```python
class A:
    __instance = None
    def __new__(cls, *args, **kwargs):
        if cls.__instance is None:
            obj = object.__new__(cls)
            cls.__instance = obj
        return cls.__instance

单例模式
```

单例模式具体分析:

单例模式是一种常用的软件设计模式。在它的核心结构中只包含一个被称为单例类的特殊类。通过单例模式可以保证系统中一个类只有一个实例而且该实例易于外界访问，从而方便对实例个数的控制并节约系统资源。如果希望在系统中某个类的对象只能存在一个，单例模式是最好的解决方案。
【采用单例模式动机、原因】
对于系统中的某些类来说，只有一个实例很重要，例如，一个系统中可以存在多个打印任务，但是只能有一个正在工作的任务；一个系统只能有一个窗口管理器或文件系统；一个系统只能有一个计时工具或ID(序号)生成器。如在Windows中就只能打开一个任务管理器。如果不使用机制对窗口对象进行唯一化，将弹出多个窗口，如果这些窗口显示的内容完全一致，则是重复对象，浪费内存资源；如果这些窗口显示的内容不一致，则意味着在某一瞬间系统有多个状态，与实际不符，也会给用户带来误解，不知道哪一个才是真实的状态。因此有时确保系统中某个对象的唯一性即一个类只能有一个实例非常重要。
如何保证一个类只有一个实例并且这个实例易于被访问呢？定义一个全局变量可以确保对象随时都可以被访问，但不能防止我们实例化多个对象。一个更好的解决办法是让类自身负责保存它的唯一实例。这个类可以保证没有其他实例被创建，并且它可以提供一个访问该实例的方法。这就是单例模式的模式动机。
【单例模式优缺点】
【优点】
一、实例控制
单例模式会阻止其他对象实例化其自己的单例对象的副本，从而确保所有对象都访问唯一实例。
二、灵活性
因为类控制了实例化过程，所以类可以灵活更改实例化过程。
【缺点】
一、开销
虽然数量很少，但如果每次对象请求引用时都要检查是否存在类的实例，将仍然需要一些开销。可以通过使用静态初始化解决此问题。
二、可能的开发混淆
使用单例对象（尤其在类库中定义的对象）时，开发人员必须记住自己不能使用new关键字实例化对象。因为可能无法访问库源代码，因此应用程序开发人员可能会意外发现自己无法直接实例化此类。
三、对象生存期
不能解决删除单个对象的问题。在提供内存管理的语言中（例如基于.NET Framework的语言），只有单例类能够导致实例被取消分配，因为它包含对该实例的私有引用。在某些语言中（如 C++），其他类可以删除对象实例，但这样会导致单例类中出现悬浮引用

### 3.9 \__item__系列

```python
class Foo:
    def __init__(self,name):
        self.name=name

    def __getitem__(self, item):
        print(self.__dict__[item])

    def __setitem__(self, key, value):
        self.__dict__[key]=value
    def __delitem__(self, key):
        print('del obj[key]时,我执行')
        self.__dict__.pop(key)
    def __delattr__(self, item):
        print('del obj.key时,我执行')
        self.__dict__.pop(item)

f1=Foo('sb')
f1['age']=18
f1['age1']=19
del f1.age1
del f1['age']
f1['name']='alex'
print(f1.__dict__)
```



