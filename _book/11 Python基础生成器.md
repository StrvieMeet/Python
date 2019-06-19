[TOC]



# Python 生成器

首先我们来看看什么是个生成器,生成器它的本质就是迭代器

在python中有以下几种方式来获取生成器

　　1.通过生成器函数

　　2.通过各种推到式来实现生成器

首先,我们先看一个很简单的函数:

```
def func():
    print(11)
    return 22
ret = func()
print(ret)

# 运行结果:
11
22
```

我们只需要修改一个地方就可以把函数变成生成器  就是将函数中的return换成yield就是生成器

## 一.定义生成器

```
def func():
    print(11)
    yield 22

func()
```

我们这样写没有任何的变化,这是为什么呢? 我们来看看函数名加括号获取到的是什么? 

为什么不会执行呢??不是函数名加括号就是调用这个函数吗? 你想的没有问题,只是因为函数体中出现了yield

咱们可以理解为,生成器是基于函数的形式变成的.

我们func()这一步是在创建一个生成器,然后我们就可以赋值到别得变量中,然后进行使用了

```
def func():

    print(11)

    yield 22

ret = func()

print(ret)

# 运行结果:

<generator object func at 0x000001A575163888>
```

获取的这个生成器.如何使用呢???

回想下迭代器是怎么使用的,再想想生成器的本质就是迭代器.我们是不是就可以直接使用迭代器的方式直接使用生成器啦

```
def func():
     print("111")
     yield 222

gener = func() # 这个时候函数不会执⾏. ⽽是获取到⽣成器
ret = gener.__next__() # 这个时候函数才会执⾏. yield的作⽤和return⼀样. 也是返回数据
print(ret)

结果:
111
222
```

那么我们可以看到,yield和return的效果是一样的,但是还是有点区别

　　yield是分段来执行一个函数,yield可以出现多次

　　return是直接停止这个函数,return可以出现多次但是只会执行到第一个就结束了

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

结果:
111
222
333
444
```

当程序运行完最后一个yield,那么后面继续运行\_\_next\_\_()程序会报错

好了生成器我们认识了,生成器有什么作用呢?

### 1.1 生成器的作用

我们来看一下这个需求,老男孩向楼下好适口的老板订购了10000个包子.好适口老板也实在一下就全部都做出来了　　

```
def eat():
    lst = []
    for i in range(1,10000):
        lst.append('包子'+str(i))
    return lst
e = eat()
print(e)
```

这样做是没有问题,但是我们目前这么点人吃不完这么多,只能先放到一个地方,过会在吃的时候包子就凉了.这样也不太好

最后是老板能够咱们吃一个他做一个.这样我们就不用考虑没地方和包子凉的问题了,咱们实现一个吃一个做一个

```
def eat():

    for i in range(1,10000):

        yield '包子'+str(i)

e = eat()

print(e.__next__())

print(e.__next__())

print(e.__next__())

print(e.__next__())

print(e.__next__())

print(e.__next__())
```

上下的区别: 第一种是直接把包子都拿来,很占内存也就是很占咱们的位置,第二种使用生成器,想吃就拿一个.吃多少个包多少个.生成器是一个一个的,一直向下进行,不能向上.\_\_next\_\_()到哪,指针就指到哪儿.下一次继续就获取指针指向的值

对比显示:生成器的好处是节省内存

我们再来看一个和\_\_next\_\_类似的东西send()

send和\_\_next\_\_()一样都可以让生成器执行到下一个yield

```
def eat():
    for i in range(1,10000):
        a = yield '包子'+str(i)
        print('a is',a)

        b = yield '窝窝头'
        print('b is', b)
e = eat()
print(e.__next__())
print(e.send('大葱'))
print(e.send('大蒜'))
```

![1548406572884](C:\Users\by\Desktop\大纲\assets\1548406572884.png)

send是将括号中的内容传给了上一yield,然后yield接收的值就可以赋值给变量

send和\_\_next\_\_()区别:

send  和 next()都是让生成器向下走一次

send可以给上一个yield的位置传递值, 在第一次执行生成器的时候不能直接使用send(),但是可以使用send(None)

生成器可以for循环来循环获取内部元素:

```
def func():
    yield 1
    yield 2
    yield 3
    yield 4
    yield 5
f = func()
for i in f:
    print(i)
```

## 二.yield from

在python3中提供一种可以直接把可迭代对象中的每一个数据作为生成器的结果进行返回

```
def func():
    lst = ['卫龙','老冰棍','北冰洋','牛羊配']
    yield from lst
g = func()
for i in g:
    print(i)
```

### 2.1 小坑

有个小坑,yield from 是将列表中的每一个元素返回,所以 如果写两个yield from 并不会产生交替的效果

```
def func():
    lst1 = ['卫龙','老冰棍','北冰洋','牛羊配']
    lst2 = ['馒头','花卷','豆包','大饼']
    yield from lst1
    yield from lst2
    
g = func()
for i in g:
    print(i)
```

## 三.推导式

### 3.1 列表推导式

列表推导式,生成器表达式以及其他推导式,首先我们先看一下这样的代码,给出一个列表,通过循环,想列表中添加1~10:

```
li = []
for i in range(10):
    li.append(i)
print(li)
```

我们换成列表推导式是什么样的,来看看:

列表推导式的常⽤写法:  

[结果 for 变量 in 可迭代对象] 

```
ls = [i for i in range(10)]

print(ls)
```

列表推导式是通过⼀行来构建你要的列表, 列表推导式看起来代码简单. 但是出现错误之后很难排查.   

例. 从python1期到python18期写入列表lst:

```
lst = ['python%s' % i for i in range(1,19)]
print(lst)
```

### 3.2 筛选模式

[结果 for 变量 in 可迭代对象 if 条件]

```
lst = [i for i in range(100) if i %2 == 0]
print(lst)
```

### 3.3 生成器推导式

生成器表达式和列表推导式的语法基本上一样的,只是把[]换成()

```
gen = (i for i in range(10))
print(gen)
# 结果: <generator object <genexpr> at 0x0000026046CAEBF8>
```

打印的结果就是一个生成器,我们可以使用for循环来循环这个生成器

```
gen = ("第%s次" % i for i in range(10))
for i in gen:
    print(i)
```

生成器表达式也可以进行筛选

```
# 获取1-100内能被3整除的数
gen = (i for i in range(1,100) if i % 3 == 0)
for num in gen:
    print(num)


# 100以内能被3整除的数的平⽅
gen = (i * i for i in range(100) if i % 3 == 0)
for num in gen:
    print(num)


# 寻找名字中带有两个e的人的名字
names = [['Tom', 'Billy', 'Jefferson', 'Andrew', 'Wesley', 'Steven', 'Joe'],
         ['Alice', 'Jill', 'Ana', 'Wendy', 'Jennifer', 'Sherry', 'Eva']]


# 不用推导式和表达式
result = []
for first in names:
    for name in first:
        if name.count("e") >= 2:
            result.append(name)
print(result)

# 推导式
gen = (name for first in names for name in first if name.count('e') >= 2)

for i in gen:
    print(i)
```

生成器表达式和列表推导式的区别:

1.列表推导式比较耗内存,一次性加载.生成器表达式几乎不占用内存.使用的时候才分配和使用内存

2.得到的值不一样,列表推导式得到的是一个列表.生成器表达式获取的是一个生成器

举个例子:

李大锤想吃鸡蛋就上街买了一篮子的鸡蛋放家里,吃的时候拿一个吃的时候拿一个,这样就是一个列表推导式,一次性拿够占地方.

王二麻子也想吃鸡蛋,他上街却买了一只母鸡回家.等他想吃的时候就让母鸡给下鸡蛋,这样就是一个生成器.需要就给你下鸡蛋

生成器的惰性机制: 生成器只有在访问的时候才取值,说白了.你找他要才给你值.不找他要.他是不会执行的.

```
def func():
    print(111)
    yield 222
g = func()  # 生成器g

g1 = (i for i in g) # 生成器g1. 但是g1的数据来源于g

g2 = (i for i in g1)    # 生成器g2. 来源g1

# list的底层有for循环,for就是一直执行__next__() 所以可以将生成器放到list中

print(list(g))   # 获取g中的数据. 这时func()才会被执行. 打印111.获取到222. g完毕.
print(list(g1))  # 获取g1中的数据. g1的数据来源是g. 但是g已经取完了. g1 也就没有数据了
print(list(g2))  # 和g1同理理

print(next(g))
print(next(g1))
print(next(g2))   # 可以用next来验证  其实list就是将内容迭代了转换成了列表
```

这是坑,一定要注意,生成器是要值的时候才能拿值,不然就没有啦

### 3.4 字典推导式

根据名字应该也能猜到,推到出来的是字典

```
lst1 = ['jay','jj','meet']
lst2 = ['周杰伦','林俊杰','郭宝元']
dic = {lst1[i]:lst2[i] for i in range(len(lst1))}
print(dic)
```

### 3.5 集合推导式

集合推导式可以帮我们直接生成一个集合,集合的特点;无序,不重复 所以集合推导式自带去重功能

```
lst = [1,2,3,-1,-3,-7,9]
s = {abs(i) for i in lst}
print(s)
```

总结:

​    推导式有, 列表推导式, 字典推导式, 集合推导式, 没有元组推导式    

​    生成器表达式: (结果 for 变量量 in 可迭代对象 if 条件筛选)    

​    生成器表达式可以直接获取到⽣成器对象. ⽣成器对象可以直接进行for循环. ⽣成器具有惰性机制. 

​    集合推导式和字典推导式很是类似,记住一个小技巧能够快速区分那个是字典那个是集合

​    字典推导式前面的结果是有个冒号,而集合的前面结果就是单纯的结果



