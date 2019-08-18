# 一.生成器

### 1.1 初识生成器

什么是生成器？这个概念比较模糊，各种文献都有不同的理解，但是核心基本相同。生成器的本质就是迭代器，在python社区中，大多数时候都把迭代器和生成器是做同一个概念。不是相同么？为什么还要创建生成器？生成器和迭代器也有不同，唯一的不同就是：迭代器都是Python给你提供的已经写好的工具或者通过数据转化得来的，（比如文件句柄，iter([1,2,3])。生成器是需要我们自己用python代码构建的工具。最大的区别也就如此了。

### **1.2 生成器的构建方式**

在python中有三种方式来创建生成器：

1. 通过生成器函数
2. 通过生成器推导式
3. python内置函数或者模块提供（其实1,3两种本质上差不多，都是通过函数的形式生成，只不过1是自己写的生成器函数，3是python提供的生成器函数而已）

### **1.3 生成器函数**

**我们先来研究通过生成器函数构建生成器。**

首先,我们先看一个很简单的函数:

```python
def func():
    print(11)
    return 22
ret = func()
print(ret)
# 运行结果:
11
22
```

将函数中的return换成yield，这样func就不是函数了，而是一个生成器函数

```
def func():
    print(11)
    yield 22
```

我们这样写没有任何的变化,这是为什么呢? 我们来看看函数名加括号获取到的是什么?

```
def func():
    print(11)
    yield 22
ret = func()
print(ret)

# 运行结果:
<generator object func at 0x000001A575163888>
```

为什么在函数中添加了yield在调用函数的时候就发现结果不是我们预想的结果呢,是因为当我们调用函数的时候函数体里的代码会进行执行当执行到yield的关键字的时候,发现我们是想声明一个生成器.程序就会返回一个生成器给咱们

那么生成器对象如何取值呢？

之前我们说了，生成器的本质就是迭代器.迭代器如何取值，生成器就如何取值。所以我们可以直接执行**next**()来执行以下生成器

```python
def func():
     print("111")
     yield 222
gener = func() # 这个时候函数不会执⾏. ⽽是获取到⽣成器
ret = gener.__next__() # 这个时候函数才会执⾏
print(ret)  # 并且yield会将func生产出来的数据 222 给了 ret。  

结果:
111
222
```

并且我的生成器函数中可以写多个yield。

```
def func():

    print("111")

    yield 222

    print("333")

    yield 444

gener = func()

ret = gener.__next__()

print(ret)

ret2 = gener.__next__()

print(ret2)

ret3 = gener.__next__()

# 最后⼀个yield执⾏完毕. 再次__next__()程序报错
print(ret3)
```

当程序运行完最后一个yield,那么后面继续运行**next**()程序会报错，一个yield对应一个next，next超过yield数量，就会报错，与迭代器一样。

​    **yield与return的区别：**

​        return一般在函数中只设置一个，他的作用是终止函数，并且给函数的执行者返回值。

​        yield在生成器函数中可设置多个，他并不会终止函数，next会获取对应yield生成的元素。

**举例：**

我们来看一下这个需求：老男孩向楼下卖包子的老板订购了10000个包子.包子铺老板非常实在，一下就全部都做出来了　

```
def eat():

    lst = []

    for i in range(1,10000):

        lst.append('包子'+str(i))

    return lst

e = eat()

print(e)
```

这样做没有问题，但是我们由于学生没有那么多，只吃了2000个左右，剩下的8000个，就只能占着一定的空间，放在一边了。如果包子铺老板效率够高，我吃一个包子，你做一个包子，那么这就不会占用太多空间存储了，完美。

```
def eat():

    for i in range(1,10000):

        yield '包子'+str(i)

e = eat()

for i in range(200):
    next(e)
```

**这两者的区别:**

​    第一种是直接把包子全部做出来，占用内存。

​    第二种是吃一个生产一个，非常的节省内存，而且还可以保留上次的位置。

```
def eat():

    for i in range(1,10000):

        yield '包子'+str(i)

e = eat()

for i in range(200):
    next(e)
    
for i in range(300):
    next(e)
# 多次next包子的号码是按照顺序记录的。
```

### **1.4 send 方法（了解,不讲）**

接下来我们再来认识一个新的东西,send方法

```
# next只能获取yield生成的值，但是不能传递值。
def gen(name):
    print(f'{name} ready to eat')
    while 1:
        food = yield
        print(f'{name} start to eat {food}')

dog = gen('alex')
next(dog)
next(dog)
next(dog)


# 而使用send这个方法是可以的。
def gen(name):
    print(f'{name} ready to eat')
    while 1:
        food = yield 222
        print(f'{name} start to eat {food}')

dog = gen('alex')
next(dog)  # 第一次必须用next让指针停留在第一个yield后面
# 与next一样，可以获取到yield的值
ret = dog.send('骨头')
print(ret)


def gen(name):
    print(f'{name} ready to eat')
    while 1:
        food = yield
        print(f'{name} start to eat {food}')

dog = gen('alex')
next(dog)
# 还可以给上一个yield发送值
dog.send('骨头')
dog.send('狗粮')
dog.send('香肠')
```

**send和next()区别:**

​        相同点：

​            send 和 next()都可以让生成器对应的yield向下执行一次。

​            都可以获取到yield生成的值。

​        不同点：

​            第一次获取yield值只能用next不能用send（可以用send(None)）。

​            send可以给上一个yield置传递值。

### 1.5 yield from

在python3中提供一种可以直接把可迭代对象中的每一个数据作为生成器的结果进行返回

```python
# 对比yield 与 yield from 
def func():
    lst = ['卫龙','老冰棍','北冰洋','牛羊配']
    yield lst
g = func()
print(g)
print(next(g))  # 只是返回一个列表
​
def func():
    lst = ['卫龙','老冰棍','北冰洋','牛羊配']
    yield from lst
g = func()
print(g)
# 他会将这个可迭代对象(列表)的每个元素当成迭代器的每个结果进行返回。
print(next(g))
print(next(g))
print(next(g))
print(next(g))
'''
yield from ['卫龙','老冰棍','北冰洋','牛羊配'] 
等同于：
    yield '卫龙'
    yield '老冰棍'
    yield '北冰洋'
    yield '牛羊配'
```

### 1.6 yield from 小坑

```python
def func():
    lst1 = ['卫龙', '老冰棍', '北冰洋', '牛羊配']
    lst2 = ['馒头', '花卷', '豆包', '大饼']
    yield from lst1
    yield from lst2


g = func()
for i in g:
    print(i)
```

返回的结果是将第一个列表的元素全部返回后,在返回第二个列表

# **二. 推导式**

本节我们讲列表推导式,生成器表达式以及其他推导式，我认为推导式就是构建比较有规律的列表,生成器，字典等一种简便的方式。那么他如何简便呢？看下面的例题：

### **2.1列表推导式**

**这里让学生自己做一下**，首先我们先看一下这样的代码,给出一个列表,通过循环,想列表中添加1~10:

```
li = []

for i in range(10):

    li.append(i)

print(li)
```

那么按照上面的要求我们用列表推导式写一下：

```
ls = [i for i in range(10)]

print(ls)
```

怎么样？一行搞定，上面这个代码就是列表推导式，接下来我们将列表推导式进行一个分类：

**列表推导式分为两种模式：**

​    1.循环模式：[变量(加工的变量) for 变量 in iterable]

​    2.筛选模式: [变量(加工的变量) for 变量 in iterable if 条件]

当然还有多层循环的，这个我们一会就会讲到，那么我们先来看循环模式。

#### **2.1.1 循环模式**

刚才我们看到的就是循环模式，那么有同学会问到，什么叫' 加工的变量'? 这个也比较简单，接下来我们做几道题：

1. 将10以内所有整数的平方写入列表。

```
l1 = [i*i for i in range(1,11)]
print(l1)
 
```

1. 100以内所有的偶数写入列表.

```
l1 = [i for i in range(2,101,2)]
print(l1)
 
```

1. 从python1期到python24期写入列表lst

```
lst = [f'python{i}' % i for i in range(1,25)]

print(lst)
```

上面那个格式化输出的变量f'python{i}'，就是加工的变量。

上面做的那三个就是循环模式，比较简单，接下来我们研究筛选模式。

#### 2.1.2 筛选模式

筛选模式就是在上面的基础上加上一个判断条件，将满足条件的变量留到列表中。

**带着同学们做一个题：**

将这个列表中大于3的元素留下来。

```
l1 = [4, 3, 2, 6, 5, 5, 7, 8] 
print([i for i in l1 if i > 3])
```

通过我给大家的演示，大家做几道题：

1. 三十以内可以被三整除的数。

   ```
   multiples = [i for i in range(30) if i % 3 is 0]
   print(multiples)
   ```

2. 过滤掉长度小于3的字符串列表，并将剩下的转换成大写字母

   ```
   l = ['wusir', 'laonanhai', 'aa', 'b', 'taibai']
   # print([i.upper() for i in l if len(i) > 3])
   ```

3. 找到嵌套列表中名字含有两个‘e’的所有名字（**有难度**）

   ```
   names = [['Tom', 'Billy', 'Jefferson', 'Andrew', 'Wesley', 'Steven', 'Joe'],
            ['Alice', 'Jill', 'Ana', 'Wendy', 'Jennifer', 'Sherry', 'Eva']]
   
   print([name for lst in names for name in lst if name.count('e') >= 2])  
   # 注意遍历顺序，这是实现的关键
   ```

列表推导式基本上讲完了，当然今天会做一些有关列表推导式的题，让大家更加深入的了解。

#### **2.1.3 生成器表达式**

生成器表达式和列表推导式的语法上一模一样,只是把[]换成()就行了。比如将十以内所有数的平方放到一个生成器表达式中

```
gen = (i**2 for i in range(10))
print(gen)
# 结果: <generator object <genexpr> at 0x0000026046CAEBF8>
```

生成器表达式也可以进行筛选

```
# 获取1-100内能被3整除的数
gen = (i for i in range(1,100) if i % 3 == 0)
for num in gen:
    print(num)
```

**生成器表达式和列表推导式的区别:**

1. 列表推导式比较耗内存,所有数据一次性加载到内存。而生成器表达式遵循迭代器协议，逐个产生元素。
2. 得到的值不一样,列表推导式得到的是一个列表.生成器表达式获取的是一个生成器
3. 列表推导式一目了然，生成器表达式只是一个内存地址。

​    无论是生成器表达式，还是列表推导式，他只是Python给你提供了一个相对简单的构造方式，因为使用推导式非常简单，所以大多数都会为之着迷，这个一定要慎重，推导式只能构建相对复杂的并且有规律的对象，对于没有什么规律，而且嵌套层数比较多（for循环超过三层）这样就不建议大家用推导式构建。

生成器的惰性机制: 生成器只有在访问的时候才取值,说白了.你找他要才给你值.不找他要.他是不会执行的.

#### **2.1.4 其他相关的推导式（了解）**

##### **字典推导式**

根据名字应该也能猜到,推到出来的是字典

```
lst1 = ['jay','jj','meet']
lst2 = ['周杰伦','林俊杰','郭宝元']
dic = {lst1[i]:lst2[i] for i in range(len(lst1))}
print(dic)
```

**集合推导式**

集合推导式可以帮我们直接生成一个集合,集合的特点;无序,不重复 所以集合推导式自带去重功能

```
lst = [1,2,3,-1,-3,-7,9]
s = {abs(i) for i in lst}
print(s)
```

# **三. 内置函数Ⅰ**

​    本节我们讲内置函数。 首先来说，函数就是以功能为导向，一个函数封装一个功能，那么Python将一些常用的功能（比如len）给我们封装成了一个一个的函数，供我们使用，他们不仅效率高（底层都是用C语言写的），而且是拿来即用，避免重复早轮子，那么这些函数就称为内置函数，到目前为止python给我们提供的内置函数一共是68个，由于时间关系以及考虑这些函数的不同重要性我们会挑常用的重要的内置函数去讲，就是下面红色黄色背景的内置函数，剩下的内置函数你们参照着我的博客自己课下练习一下即可。

​    由于我们这没有表格的功能，我把这些内置函数进行分类：

<table><tr><td bgcolor=orange> <font color=white>黄色一带而过：all()  any()  bytes() callable() chr() complex() divmod() eval() exec() format() frozenset() globals() hash() help() id() input() int()  iter() locals() next()  oct()  ord()  pow()    repr()  round()</font></td></tr></table>
<table><tr><td bgcolor=red> <font color=wihte>红色重点讲解：abs() enumerate() filter()  map() max()  min() open()  range() print()  len()  list()  dict() str()  float() reversed()  set()  sorted()  sum()    tuple()  type()  zip()  dir() </font></font></td></tr></table>
<table><tr><td bgcolor=blue><font color=white>蓝色未来会讲： classmethod()  delattr() getattr() hasattr()  issubclass()  isinstance()  object() property()  setattr()  staticmethod()  super()
  </font></td></tr></table>

上面的黄色，红色的内置函数是在这两天讲完的（讲过的就不讲了），蓝色的讲完面向对象会给大家补充，剩余还有一些课上就不讲了，课下练习一下就可以。

eval：执行字符串类型的代码，并返回最终结果。

```
eval('2 + 2')  # 4
n=81
eval("n + 4")  # 85
eval('print(666)')  # 666
```

exec:执行字符串类型的代码。

```
s = '''
for i in [1,2,3]:
    print(i)
'''
exec(s)
```

以上两个内置函数很强大 工作中禁止使用

hash：获取一个对象（可哈希对象：int，str，Bool，tuple）的哈希值。

```
print(hash(12322))
print(hash('123'))
print(hash('arg'))
print(hash('alex'))
print(hash(True))
print(hash(False))
print(hash((1,2,3)))

'''
-2996001552409009098
-4637515981888139739
1
2528502973977326415
'''
```

help：函数用于查看函数或模块用途的详细说明。

```
print(help(list))
print(help(str.split))
```

callable：函数用于检查一个对象是否是可调用的。如果返回True，仍然可能调用失败；但如果返回False，调用对象ojbect绝对不会成功。

```
name = 'alex'
def func():
    pass
print(callable(name))  # False
print(callable(func))  # True
```

int：函数用于将一个字符串或数字转换为整型。

```
print(int())  # 0
print(int('12'))  # 12
print(int(3.6))  # 3
print(int('0100',base=2))  # 将2进制的 0100 转化成十进制。结果为 4
```

float：函数用于将整数和字符串转换成浮点数。

complex：函数用于创建一个值为 real + imag * j 的复数或者转化一个字符串或数为复数。如果第一个参数为字符串，则不需要指定第二个参数。。

```
print(float(3))  # 3.0
print(complex(1,2))  # (1+2j)
```

bin：将十进制转换成二进制并返回。

oct：将十进制转化成八进制字符串并返回。

hex：将十进制转化成十六进制字符串并返回。

```
print(bin(10),type(bin(10)))  # 0b1010 <class 'str'>
print(oct(10),type(oct(10)))  # 0o12 <class 'str'>
print(hex(10),type(hex(10)))  # 0xa <class 'str'>
```

divmod：计算除数与被除数的结果，返回一个包含商和余数的元组(a // b, a % b)。

round：保留浮点数的小数位数，默认保留整数。

pow：求x\*\*y次幂。（三个参数为x**y的结果对z取余）

```
print(divmod(7,2))  # (3, 1)
print(round(7/3,2))  # 2.33
print(round(7/3))  # 2
print(round(3.32567,3))  # 3.326
print(pow(2,3))  # 两个参数为2**3次幂
print(pow(2,3,3))  # 三个参数为2**3次幂，对3取余。
```

bytes：用于不同编码之间的转化。

```
# s = '你好'
# bs = s.encode('utf-8')
# print(bs)
# s1 = bs.decode('utf-8')
# print(s1)
# bs = bytes(s,encoding='utf-8')
# print(bs)
# b = '你好'.encode('gbk')
# b1 = b.decode('gbk')
# print(b1.encode('utf-8'))
```

ord:输入字符找当前字符编码的位置

chr:输入当前编码的位置数字找出其对应的字符

```
# ord 输入字符找该字符编码的位置
# print(ord('a'))
# print(ord('中'))

# chr 输入位置数字找出其对应的字符
# print(chr(97))
# print(chr(20013))
```

repr:返回一个对象的string形式（原形毕露）。

```
# %r  原封不动的写出来
# name = 'taibai'
# print('我叫%r'%name)

# repr 原形毕露
print(repr('{"name":"alex"}'))
print('{"name":"alex"}')
```

all：可迭代对象中，全都是True才是True

any：可迭代对象中，有一个True 就是True

```
# all  可迭代对象中，全都是True才是True
# any  可迭代对象中，有一个True 就是True
# print(all([1,2,True,0]))
# print(any([1,'',0]))
```

