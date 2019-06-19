[TOC]



# Python面向对象三大特性

## 一.封装

把很多数据封装到⼀个对象中. 把固定功能的代码封装到⼀个代码块, 函数, 对象, 打包成模块. 这都属于封装的思想. 具体的情况具体分析. 比如. 你写了⼀个很⽜B的函数. 那这个也可以被称为封装. 在⾯向对象思想中. 是把⼀些看似⽆关紧要的内容组合到⼀起统⼀进⾏存储和使⽤. 这就是封装. 

封装，顾名思义就是将内容封装到某个地方，以后再去调用被封装在某处的内容。

所以，在使用面向对象的封装特性时，需要：

- 将内容封装到某处
- 从某处调用被封装的内容

**第一步：将内容封装到某处**

![img](https://images0.cnblogs.com/blog2015/425762/201508/271641407509817.jpg)

 self 是一个形式参数，当执行 obj1 = Foo('wupeiqi', 18 ) 时，self 等于 obj1

​                              当执行 obj2 = Foo('alex', 78 ) 时，self 等于 obj2

所以，内容其实被封装到了对象 obj1 和 obj2 中，每个对象中都有 name 和 age 属性，在内存里类似于下图来保存。

![img](https://images0.cnblogs.com/blog2015/425762/201508/271653303446704.jpg)

**第二步：从某处调用被封装的内容**

调用被封装的内容时，有两种情况：

- 通过对象直接调用
- 通过self间接调用

1、通过对象直接调用被封装的内容

上图展示了对象 obj1 和 obj2 在内存中保存的方式，根据保存格式可以如此调用被封装的内容：对象.属性名

```
class Foo:
 
    def __init__(self, name, age):
        self.name = name
        self.age = age
 
obj1 = Foo('wupeiqi', 18)
print obj1.name    # 直接调用obj1对象的name属性
print obj1.age     # 直接调用obj1对象的age属性
 
obj2 = Foo('alex', 73)
print obj2.name    # 直接调用obj2对象的name属性
print obj2.age     # 直接调用obj2对象的age属性
```

2、通过self间接调用被封装的内容

执行类中的方法时，需要通过self间接调用被封装的内容

```
class Foo:
  
    def __init__(self, name, age):
        self.name = name
        self.age = age
  
    def detail(self):
        print self.name
        print self.age
  
obj1 = Foo('wupeiqi', 18)
obj1.detail()  # Python默认会将obj1传给self参数，即：obj1.detail(obj1)，所以，此时方法内部的 self ＝ obj1，即：self.name 是 wupeiqi ；self.age 是 18
  
obj2 = Foo('alex', 73)
obj2.detail()  # Python默认会将obj2传给self参数，即：obj1.detail(obj2)，所以，此时方法内部的 self ＝ obj2，即：self.name 是 alex ； self.age 是 78
```

**综上所述，对于面向对象的封装来说，其实就是使用构造方法将内容封装到 对象 中，然后通过对象直接或者self间接获取被封装的内容。**

## 二.继承

⼦类可以⾃动拥有⽗类中除了私有属性外的其他所有内容. 说⽩了, ⼉⼦可以随便⽤爹的东⻄. 但是朋友们, ⼀定要认清楚⼀个事情. 必须先有爹, 后有⼉⼦. 顺序不能乱, 在python中实现继承非常简单. 在声明类的时候, 在类名后⾯添加⼀个⼩括号,就可以完成继承关系. 那么什么情况可以使⽤继承呢? 单纯的从代码层⾯上来看. 两个类具有相同的功能或者特征的时候. 可以采⽤继承的形式. 提取⼀个⽗类, 这个⽗类中编写着两个类相同的部分. 然后两个类分别取继承这个类就可以了. 这样写的好处是我们可以避免写很多重复的功能和代码. 如果从语义中去分析的话. 会简单很多. 如果语境中出现了x是⼀种y. 这时, y是⼀种泛化的概念. x比y更加具体. 那这时x就是y的⼦类. 比如. 猫是⼀种动物. 猫继承动物. 动物能动. 猫也能动. 这时猫在创建的时候就有了动物的"动"这个属性. 再比如, ⽩骨精是⼀个妖怪. 妖怪天⽣就有⼀个比较不好的功能叫"吃⼈", ⽩骨精⼀出⽣就知道如何"吃⼈". 此时 ⽩骨精继承妖精.

## 三.多态

同⼀个对象, 多种形态. 这个在python中其实是很不容易说明⽩的. 因为我们⼀直在⽤. 只是没有具体的说. 比如. 我们创建⼀个变量a = 10 , 我们知道此时a是整数类型. 但是我们可以通过程序让a = "alex", 这时, a⼜变成了字符串类型. 这是我们都知道的. 但是, 我要告诉你的是. 这个就是多态性. 同⼀个变量a可以是多种形态。

多态，同一个对象，多种形态。python默认支持多态。

```
# 在java或者c#定义变量或者给函数传值必须定义数据类型，否则就报错。

def func(int a):
    print('a必须是数字')
    
# 而类似于python这种弱定义类语言,a可以是任意形态（str,int,object等等）。
def func(a):
    print('a是什么都可以')
    
# 再比如：
class F1:
    pass


class S1(F1):
    
    def show(self):
        print 'S1.show'


class S2(F1):
    
    def show(self):
        print 'S2.show'


# 由于在Java或C#中定义函数参数时，必须指定参数的类型
# 为了让Func函数既可以执行S1对象的show方法，又可以执行S2对象的show方法，所以，定义了一个S1和S2类的父类
# 而实际传入的参数是：S1对象和S2对象

def Func(F1 obj):
"""Func函数需要接收一个F1类型或者F1子类的类型"""

    print obj.show()
    

s1_obj = S1()
Func(s1_obj)  # 在Func函数中传入S1类的对象 s1_obj，执行 S1 的show方法，结果：S1.show

s2_obj = S2()
Func(s2_obj)  # 在Func函数中传入Ss类的对象 ss_obj，执行 Ss 的show方法，结果：S2.show

Python伪代码实现Java或C  # 的多态
```

鸭子类型

```
python中有一句谚语说的好，你看起来像鸭子，那么你就是鸭子。
对于代码上的解释其实很简答：
class A:
    def f1(self):
        print('in A f1')
    
    def f2(self):
        print('in A f2')


class B:
    def f1(self):
        print('in A f1')
    
    def f2(self):
        print('in A f2')
        
obj = A()
obj.f1()
obj.f2()

obj2 = B()
obj2.f1()
obj2.f2()
# A 和 B两个类完全没有耦合性，但是在某种意义上他们却统一了一个标准。
# 对相同的功能设定了相同的名字，这样方便开发，这两个方法就可以互成为鸭子类型。

# 这样的例子比比皆是：str  tuple list 都有 index方法，这就是统一了规范。
# str bytes 等等 这就是互称为鸭子类型。
```

## 四.类的约束

⾸先, 你要清楚. 约束是对类的约束. 

用一个例子说话：

公司让小明给他们的网站完善一个支付功能，小明写了两个类，如下：

```
class QQpay:
    def pay(self,money):
        print('使用qq支付%s元' % money)

class Alipay:
    def pay(self,money):
        print('使用阿里支付%s元' % money)

a = Alipay()
a.pay(100)

b = QQpay()
b.pay(200)
```

但是上面这样写不太放方便，也不合理，老大说让他整改，统一一下付款的方式，小明开始加班整理：

```
class QQpay:
    def pay(self,money):
        print('使用qq支付%s元' % money)

class Alipay:
    def pay(self,money):
        print('使用阿里支付%s元' % money)

def pay(obj,money):  # 这个函数就是统一支付规则，这个叫做： 归一化设计。
    obj.pay(money)

a = Alipay()
b = QQpay()

pay(a,100)
pay(b,200)
```

写了半年的接口，小明终于接了大项目了，结果公司没品位，招了一个野生的程序员春哥接替小明的工作，老大给春哥安排了任务，让他写一个微信支付的功能：

```
class QQpay:
    def pay(self,money):
        print('使用qq支付%s元' % money)

class Alipay:
    def pay(self,money):
        print('使用阿里支付%s元' % money)

class Wechatpay:  # 野生程序员一般不会看别人怎么写，自己才是最好，结果......
    def fuqian(self,money):
        print('使用微信支付%s元' % money)

def pay(obj,money):
    obj.pay(money)

a = Alipay()
b = QQpay()

pay(a,100)
pay(b,200)

c = Wechatpay()
c.fuqian(300)
```

结果春哥，受惩罚了，限期整改，那么春哥，发奋图强，python的相关资料，重新梳理的代码：

```
class Payment: 
　　""" 此类什么都不做，就是制定一个标准，谁继承我，必须定义我里面的方法。
   """
    def pay(self,money):pass

class QQpay(Payment):
    def pay(self,money):
        print('使用qq支付%s元' % money)

class Alipay(Payment):
    def pay(self,money):
        print('使用阿里支付%s元' % money)

class Wechatpay(Payment):
    def fuqian(self,money):
        print('使用微信支付%s元' % money)


def pay(obj,money):
    obj.pay(money)

a = Alipay()
b = QQpay()

pay(a,100)
pay(b,200)

c = Wechatpay()
c.fuqian(300)
```

但是，这样还会有问题，如果再来野生程序员，他不看其他的支付方式，也不知道为什么继承的类中要定义一个没有意义的方法，所以他会是会我行我素：

```
class Payment: 
　　""" 此类什么都不做，就是制定一个标准，谁继承我，必须定义我里面的方法。
   """
    def pay(self,money):pass

class QQpay(Payment):
    def pay(self,money):
        print('使用qq支付%s元' % money)

class Alipay(Payment):
    def pay(self,money):
        print('使用阿里支付%s元' % money)

class Wechatpay(Payment):
    def fuqian(self,money):
        print('使用微信支付%s元' % money)


def pay(obj,money):
    obj.pay(money)

a = Alipay()
b = QQpay()

pay(a,100)
pay(b,200)

c = Wechatpay()
c.fuqian(300)
```

所以此时我们要用到对类的约束，对类的约束有两种：

1.提取⽗类. 然后在⽗类中定义好⽅法. 在这个⽅法中什么都不⽤⼲. 就抛⼀个异常就可以了. 这样所有的⼦类都必须重写这个⽅法. 否则. 访问的时候就会报错. 

2.使⽤元类来描述⽗类. 在元类中给出⼀个抽象⽅法. 这样⼦类就不得不给出抽象⽅法的具体实现. 也可以起到约束的效果.

先用第一种方式解决：

```
class Payment:
    """
    此类什么都不做，就是制定一个标准，谁继承我，必须定义我里面的方法。
    """
    def pay(self,money):
        raise Exception("你没有实现pay方法")

class QQpay(Payment):
    def pay(self,money):
        print('使用qq支付%s元' % money)

class Alipay(Payment):
    def pay(self,money):
        print('使用阿里支付%s元' % money)

class Wechatpay(Payment):
    def fuqian(self,money):
        print('使用微信支付%s元' % money)


def pay(obj,money):
    obj.pay(money)

a = Alipay()
b = QQpay()
c = Wechatpay()
pay(a,100)
pay(b,200)
pay(c,300)
```

第二种方式：引入抽象类的概念处理

```
from abc import ABCMeta,abstractmethod
class Payment(metaclass=ABCMeta):    # 抽象类 接口类  规范和约束  metaclass指定的是一个元类
    @abstractmethod
    def pay(self):pass  # 抽象方法

class Alipay(Payment):
    def pay(self,money):
        print('使用支付宝支付了%s元'%money)

class QQpay(Payment):
    def pay(self,money):
        print('使用qq支付了%s元'%money)

class Wechatpay(Payment):
    # def pay(self,money):
    #     print('使用微信支付了%s元'%money)
    def recharge(self):pass

def pay(a,money):
    a.pay(money)

a = Alipay()
a.pay(100)
pay(a,100)    # 归一化设计：不管是哪一个类的对象，都调用同一个函数去完成相似的功能
q = QQpay()
q.pay(100)
pay(q,100)
w = Wechatpay()
pay(w,100)   # 到用的时候才会报错



# 抽象类和接口类做的事情 ：建立规范
# 制定一个类的metaclass是ABCMeta，
# 那么这个类就变成了一个抽象类(接口类)
# 这个类的主要功能就是建立一个规范
```

**总结: 约束. 其实就是⽗类对⼦类进⾏约束. ⼦类必须要写xxx⽅法. 在python中约束的⽅式和⽅法有两种:**

**1. 使⽤抽象类和抽象⽅法, 由于该⽅案来源是java和c#. 所以使⽤频率还是很少的**

**2. 使⽤⼈为抛出异常的⽅案. 并且尽量抛出的是NotImplementError. 这样比较专业, ⽽且错误比较明确.(推荐)**

## 五.super

### **super是严格按照类的继承顺序执行！！！**

```
class A:
    def f1(self):
        print('in A f1')
    
    def f2(self):
        print('in A f2')


class Foo(A):
    def f1(self):
        super().f2()
        print('in A Foo')
        
        
obj = Foo()
obj.f1()

super可以下一个类的其他方法
```

super()严格按照类的mro顺序执行

```
class A:
    def f1(self):
        print('in A')

class Foo(A):
    def f1(self):
        super().f1()
        print('in Foo')

class Bar(A):
    def f1(self):
        print('in Bar')

class Info(Foo,Bar):
    def f1(self):
        super().f1()
        print('in Info f1')

obj = Info()
obj.f1()

'''
in Bar
in Foo
in Info f1
'''
print(Info.mro())  # [<class '__main__.Info'>, <class '__main__.Foo'>, <class '__main__.Bar'>, <class '__main__.A'>, <class 'object'>]
```

