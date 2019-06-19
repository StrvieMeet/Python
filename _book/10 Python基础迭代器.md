[TOC]

# Python 迭代器

## 一. 函数名的运用

函数名是一个变量, 但它是一个特殊的变量, 与括号配合可以执行函数的变量

### 1.1.函数名的内存地址

```
def func():   
    print("呵呵")
print(func)

结果: <function func at 0x1101e4ea0>
```

### 1.2 函数名可以赋值给其他变量

```
def func():   
    print("呵呵")
    print(func)
a = func    # 把函数当成一个值赋值给另一个变量

a()     # 函数调用 func()
```

### 1.3. 函数名可以当做容器类的元素

```
def func1():   
    print("呵呵")
def func2():   
    print("呵呵")
def func3():   
    print("呵呵")
def func4():  
     print("呵呵")

lst = [func1, func2, func3]
for i in lst:  
     i()
```

### 1.4.函数名可以当做函数的参数

```
def func():   
    print("吃了么")
def func2(fn):   
    print("我是func2")   
    fn()    # 执行传递过来的fn   
    print("我是func2")
func2(func)     # 把函数func当成参数传递给func2的参数fn.
```

### 1.5. 函数名可以作为函数的返回值

```
def func_1():   
    print("这里是函数1")   
    def func_2():       
        print("这里是函数2")   
    print("这里是函数1")   
    return func_2
fn = func_1()  
# 执行函数1.  函数1返回的是函数2, 这时fn指向的就是上面函数2
fn()    # 执行func_2函数
```

## 二.闭包

什么是闭包?  闭包就是内层函数, 对外层函数(非全局)的变量的引用. 叫闭包

```
def func1():
    name = "alex"
    def func2():
        print(name)
        # 闭包
    func2()
func1()
# 结果: alex
```

### 2.1 检测闭包

我们可以使用\_\_closure\_\_ 来检测函数是否是闭包. 使用函数名.\_\_closure\_\_返回cell就是
闭包. 返回None就不是闭包

```
def func1():
    name = "alex"
    def func2():
        print(name)
    func2()
    print(func2.__closure__)

func1()

结果:
alex
(<cell at 0x0000020077EFC378: str object at 0x00000200674DC340>,)
```

`返回的结果不是None就是闭包`

这样写没有问题,但是有个问题就是这个里边的函数只能先执行了func1才能执行func2,我想在外边调用怎么办呢?

我们可不可以将里边的函数名当做参数返回给调用者啊

```
def outer():   
    name = "alex"   
    # 内部函数   
    def inner():       
        print(name)   
    return inner
fn = outer()   # 访问外部函数, 获取到内部函数的函数地址
fn()    # 访问内部函数
```

这样就实现了外部访问,那如果多层嵌套呢?很简单,只需要一层一层的往外层返回就行了

```
def func1():  
    def func2(): 
        s = '嘿嘿'
        def func3():       
            print(s)      
        return func3 
    return func2
func1()()()
```

这样我们在外界可以访问内部函数. 那这个时候内部函数访问的时间和时机就不一定了, 因为在外部, 我可以选择在任意的时间去访问内部函数. 这 个时候. 想一想. 我们之前说过, 如果一个函数执行完毕. 则这个函数中的变量以及局部命名空间中的内容都将会被销毁.  在闭包中. 如果变量被销毁了. 那内部函数将不能正常执行. 所 以. python规定. 如果你在内部函数中访问了外层函数中的变量. 那么这个变量将不会消亡. 将会常驻在内存中. 也就是说. 使用闭包, 可以保证外层函数中的变量在内存中常驻. 这样做有什么好处呢? 非常大的好处. 闭包的作用就是让一个变量能够常驻内存,供后面的程序使用

## 三 .迭代器

我们之前一直在用可迭代对象进行操作,那么到底什么是可迭代对象.我们现在就来讨论讨论可迭代对象.首先我们先回顾下我们

熟知的可迭代对象有哪些:

str  list   tuple  dic  set  那为什么我们称他们为可迭代对象呢?因为他们都遵循了可迭代协议,那什么又是可迭代协议呢.首先我们先看一段代码:

正确的代码:

```
s = 'abc'
for i in s:
    print(i)

结果:
a
b
c
错误的代码:
for i in 123:
    print(i)


结果
Traceback (most recent call last):
  File "D:/python_object/二分法.py", line 62, in <module>
    for i in 123:
TypeError: 'int' object is not iterable
```

注意看报错信息,报错信息中有这样一句话: 'int' object is not iterable 翻译过来就是整数类型对象是不可迭代的.

iterable表示可迭代的.表示可迭代协议 那么如何进行验证你的数据类型是否符合可迭代协议.我们可以通过dir函数来查看类中定义好的所有方法

```
a = 'abc'
print(dir(a))  # dir查看对象的方法和函数

# 在打印结果中寻找__iter__ 如果存在就表示当前的这个类型是个可迭代对象
```

我们刚刚测了字符串中是存在 \_\_iter\_\_ 的,那我们来看看  列表,元祖,字典.集合中是不是有存在\_\_iter\_\_

```
# 列表

lst = [1,2]
print(dir(lst))


# 元祖

tuple = (1,2)
print(dir(tuple))


# 字典

dic = {'a':1,'b':2}
print(dir(dic))



# 集合

se = {1,2,3,4,4}
print(dir(se))
```

是不是发现以上都有\_\_iter\_\_并且还很for循环啊,其实也可以这么说可以for循环的就有\_\_iter\_\_方法,包括range

```
print(dir(range))
```

这是查看一个对象是否是可迭代对象的第一种方法,我们还可以通过isinstence()函数来查看一个对象是什么类型的

```
l = [1,2,3]

l_iter = l.__iter__()

from collections import Iterable
from collections import Iterator

print(isinstance(l,Iterable)) #True             #查看是不是可迭代对象
print(isinstance(l,Iterator)) #False            #查看是不是迭代器
print(isinstance(l_iter,Iterator)) #True       
print(isinstance(l_iter,Iterable)) #True
```

通过上边的我们可以确定.如果对象中有\_\_iter\_\_函数,那么我们认为这个对象遵守了可迭代协议.就可以获取到相应的迭代器

.这里的\_\_iter\_\_是帮助我们获取到对象的迭代器.我们使用迭代器中的\_\_next\_\_()来获取到一个迭代器的元素,那么我们之前所讲的

### 3.1 for循环机制

for的工作原理到底是什么? 继续向下看:

```
s = "我爱北京天安⻔"

c = s.__iter__() # 获取迭代器
print(c.__next__()) # 使⽤迭代器进⾏迭代. 获取⼀个元素 我
print(c.__next__()) # 爱
print(c.__next__()) # 北
print(c.__next__()) # 京
print(c.__next__()) # 天
print(c.__next__()) # 安
print(c.__next__()) # ⻔
print(c.__next__()) # StopIteration
```

for循环是不是也可以,并且还不报错啊,其实上边就是for的机制,

我们使用while循环和迭代器来模拟for循环: 必须要会

```
lst = [6,5,4]

l = lst.__iter__()

while True:
    try:
        i = l.__next__()
        print(i)
    except StopIteration
        break
```

注意: 迭代器不能反复,只能向下执行,并且是一次性的.获取过了就不能在获取了

### 3.2 总结 

​        Iterable: 可迭代对象. 内部包含\_\_iter\_\_()函数        

​        Iterator: 迭代器. 内部包含\_\_iter\_\_() 同时包含\_\_next__().        

迭代器的特点:            

​       1. 节省内存.            

​        2. 惰性机制            

​        3. 不能反复, 只能向下执行.    

我们可以把要迭代的内容当成子弹. 然后呢. 获取到迭代器\_\_iter\_\_(), 就把子弹都装在弹夹中.  然后发射就是\_\_next\_\_()把每一个子弹(元素)打出来. 也就是说, for循环的时候.一开始的 时候是\_\_iter\_\_()来获取迭代器. 后面每次获取元素都是通过__next__()来完成的. 当程序遇到 StopIteration将结束循环. 

