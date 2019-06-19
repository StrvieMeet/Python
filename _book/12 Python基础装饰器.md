[TOC]



# Python 装饰器及内置函数 

## 一.装饰器

我们到了新的阶段了,也是逃不过的一劫,装饰器,有的兄弟们估计听说过,这可是一个神奇的东西,

在认识它之前我们先来回顾一下闭包

```
def func1():
    name = "alex"
    def func2():
        print(name)
        # 闭包
    func2()
func1()
```

这就是闭包,那刚刚说一个很神奇的东西,为什么又说闭包了呢? 机智的铁子们已经猜到,装饰器和闭包是有点关系的,是滴没错.

那什么是装饰器呢?装饰器是干什么的呢?

现在你在公司，领导让你写一个函数，来测试另一个函数的执行效率，你怎么做？

```
先引入时间概念。
import time
def func():
    print('嘻嘻更健康')
import time
start_time = time.time()
time.sleep(0.1)
func()
end_time = time.time()
print('----> 执行效率%s'%(end_time - start_time))
```

上面你已经写完了，但是你应该放在函数中，这样减少重复代码，可读性好，ok继续做。

```
def func():
    print('嘻嘻更健康')
def timmer(f):
    start_time = time.time()
    time.sleep(0.1)
    f()
    end_time = time.time()
    print('----> 执行效率%s'%(end_time - start_time))
```

好你又写完了，但是执行之前的函数只是func(),而你写玩了这个之后，还得加一步timmer(func),如果要是领导让你测试500个函数的执行效率呢？好，你又进一步改，如下

```
func()
f1 = func  # func
func = timmer  # timmer
func(f1)
将他的执行结果改了一下，这样看似func(f1)与原来的调用差不多，但是加了好几步，而且添加了f1参数。。。
```

现在你请教了我，我说来，写个装饰器就能解决。

```
def timmer(f):  #小花
    def inner():  # 小红
        start_time = time.time()
        time.sleep(0.1)
        f()
        end_time = time.time()
        print('----> 执行效率%s' % (end_time - start_time))
    return inner
    
def func(): #小刚
	print("is func")
func = timmer(func)  # inner
func() # inner()
```

这样，就写好了，这是最简单的装饰器，装饰任何函数，只需要加一句func = timmer(func) 

肯定有人在想,这一堆鬼东西是什么啊,慢 别急往下看.兄弟我说的是往下看文章,不是看你下边

func函数是小刚,timmer函数是小花,inner函数是小红.小花和小红是非常好的闺蜜

小刚对小红一直暗生情愫,直到有一天憋不住了想和小红说但是,又想知道小红对他什么看法所以小刚找到了小花

然后把他的想法和小花说了,小花然后问小红你觉得小刚怎么样.小红说很好啊,我还挺喜欢他的,然后小花就把小红的话告诉了小刚,小刚听了这句话很是激动,然后就直接去找小红和小红进行表白了.

```
#简单的装饰器
def func():
    print('嘻嘻更健康')
def timmer(f):
    def inner():
        start_time = time.time()
        time.sleep(0.1)
        f()
        end_time = time.time()
        print('----> 执行效率%s' % (end_time - start_time))
    return inner
func = timmer(func)  # inner
func() # inner()
```

但是Python认为你这个还是不简单，所以Python给你提供了一个更见的方式就是语法糖。

```
#语法糖 @  

def timmer(f):

    def inner():

        start_time = time.time()

        time.sleep(0.1)

        f()

        end_time = time.time()

        print('----> 执行效率%s' % (end_time - start_time))

    return inner

@timmer  # func = timmer(func) 
```

语法糖的拆解

@装饰器函数 == 重新定义被装饰函数=装饰器函数（被装饰函数）

```
def func():
    print('嘻嘻更健康')
func() # inner()
```

## 二.内置函数


什么是内置函数? 就是python给你提供的. 拿来直接用的函数, 比如print., input等等.

截止 到python版本3.6.2 python一共提供了68个内置函数. 有 一些我们已经用过了. 有一些还没有用过. 还有一些需要学完了面向对象才能继续学习的.

今 天我们就认识一下python的内置函数.

![img](http://crm.pythonav.com/media/uploads/2018/11/28/IMAGE_VeRf3JP.PNG)

### 2.1 作用域相关  

#### ​    2.1.1 locals()      返回当前作⽤用域中的名字

#### ​    2.1.2 globals()    返回全局作⽤用域中的名字

### 2.2 迭代器相关

#### ​     2.2.1 range()       生成数据    

#### ​     2.2.2.next()         迭代器向下执⾏一次, 内部实际使用了__next__()方法返回迭代器的下一个项目    

#### ​     2.2.3  iter()           获取迭代器, 内部实际使用的是__iter__()方法来获取迭代器

### 2.3 字符串类型代码的执行

#### 2.3.1 eval() 执行部分字符串类型的代码,并返回最终结果

```
print(eval("2+2"))
# 4
n = 8
print(eval("2+n"))
# 10

def func():
    print(666)

eval("func()")
# 666
```

#### 2.3.2 exec() 执行字符串类型的代码

```
msg = '''
def func():
    print('有计划没行动等于零')
    
func()
'''
exec(msg)
```

以上这两个在公司开发中禁止使用,如果里边出现del就会出现很大的问题

### 2.4 输入和输出相关

#### 2.4.1 input()    获取用户输入的内容 

#### 2.4.2 print()    打印输出

```
print('你好','我好')    
print('你好','我好',sep='|')

结果:
你好 我好
你好|我好
```

sep是将多个元素进行修改 默认的是空格

```
print('你好')
print('我好')

print('你好',end='')
print('我好')
```

end默认是\n 这就是我们为什么使用print的时候会出现换行,end的值修改成了空字符串

### 2.5 内存相关   

#### 2.5.1 hash()    获取到对象的哈希值(int, str, bool, tuple)

```
print(hash('123'))
结果:
-6822401661081700707
```

这样是求出数据结构,如果能够获取到哈希值就是可以当做字典的键

#### 2.5.2 id()      获取到对象的内存地址 

### 2.6 文件操作相关

#### 2.6.1 open()    用于打开一个文件, 创建一个文件句柄

### 2.7 帮助  

#### 2.7.1 help()    函数用于查看函数或模块用途的详细说明 

```
help(print)

结果:
Help on built-in function print in module builtins:

print(...)
    print(value, ..., sep=' ', end='\n', file=sys.stdout, flush=False)
    
    Prints the values to a stream, or to sys.stdout by default.
    Optional keyword arguments:
    file:  a file-like object (stream); defaults to the current sys.stdout.
    sep:   string inserted between values, default a space.
    end:   string appended after the last value, default a newline.
    flush: whether to forcibly flush the stream.


Process finished with exit code 0
```

### 2.8 调用相关   

2. #### 8.1 callable()  用于检查一个对象是否是可调用的. 如果返回True, object有可能调用失败, 但如果返回False. 那调用绝对不会成功 

```
print(callable(print))
结果:
True
```

### 2.9 查看内置属性    

#### ​    2.9.1 dir()查看对象的内置属性

方法.访问的是对象中的__dir__()⽅方法 

```
print(dir(list))
结果:
['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']
```

### 2.10 基础数据类型相关    

#### 2.10.1 数字相关

​        bool()  将给定的数据转换成bool值. 如果不给值. 返回False        
​        int()     将给定的数据转换成int值. 如果不给值, 返回0        
​        ﬂoat()  将给定的数据转换成ﬂoat值. 也就是小数        
​        complex()  创建一个复数. 第一个参数为实部, 第二个参数为虚部. 或者第一个参数直接 用字符串来描述复数    

#### 2.10.2 数学运算

​        abs()         返回绝对值

```
print(abs(-1))
结果:
1
```

​        divmod()     返回商和余数        

```
print(divmod(15,2))
结果:
(7, 1)
```

​        round()        四舍五入 

```
print(round(15.1111,2))  # 保留2位小数
结果:
15.11
```

​        pow(a, b)    求a的b次幂, 如果有三个参数. 则求完次幂后对第三个数取余

```
print(pow(15,2,3))
结果:
0
```

​        sum()        求和

```
print(sum([12,3,4]))  #sum里边的参数是一个可迭代对象
结果:
19
```

​        min()    求最小值 

```
print(min([12,3,4]))  # 寻找最小的数字
结果:
3
```

​        max()    求最大值

```
print(max([12,3,4]))  # 寻找最大的数字
结果:
12
```

#### 2.10 .3数据结构相关

​    列表和元组:        
​        list()        将一个可迭代对象转换成列表        
​        tuple()       将一个可迭代对象转换成元组        
​        reversed()    将一个序列翻转, 返回翻转序列的迭代器

reversed 示例:

```
l = reversed('你好')  # l 获取到的是一个生成器
print(list(l))
```

字符串相关:    
    str()        将数据转化成字符串       
    format()     与具体数据相关, 用于计算各种小数, 精算等 

```
print(format('meet','>20'))   # 右对齐
print(format('meet','<20'))   # 左对齐
print(format('meet','^20'))   # 居中
```

数值

```
#数值

print(format(3,'b'))    # 二进制

print(format(97,'c'))   # 转换成unicodezif

print(format(11,'d'))   #十进制

print(format(56))     #和d一样

print(format(11,'n'))   #十进制

print(format(11,'o'))   #八进制

print(format(11,'x'))  # 十六进制(小写字母)

print(format(11,'X'))  # 十六进制(大写字母)



# 浮点数

print(format(1234567890,'e'))  #科学计算法,默认使用6位

print(format(123456789,'0.2e'))# 科学计算,保留2位小数(小写)

print(format(123456789,'0.2E'))# 科学计算,保留2位小数(大写)

print(format(1.23456789,'f')) #小数点计数法,保留6位小数

print(format(1.23456789,'0.2f')) # 小数点计数法,保留2位数

print(format(1.23456789,'0.10f')) # 小数点计数法,保留2位数

print(format(1.23456789e+1000,'F')) # 小数点计数法
```

bytes()  把字符串转换成bytes类型

```
s = '你好武大'

bs = s.encode('utf-8')

print(bs)

结果:b'\xe4\xbd\xa0\xe5\xa5\xbd\xe6\xad\xa6\xe5\xa4\xa7'

s1 = bs.decode('utf-8')

print(s1)

结果: 你好武大


s = '你好'
bs = bytes(s,encoding='utf-8')
print(bs)
# 将字符串转换成字节

bs1 = str(bs,encoding='utf-8')
print(bs1)
# 将字节转换成字符串
```

repr()    返回一个对象的官方表示形式

```
# repr 输出一个字符串的官方表示形式. 
print(repr('大家好,\n \t我叫周杰伦')) print('大家好我叫周杰伦') 

# %r  
%r用的就是repr 

name = 'taibai' 
print('我叫%r' % name)
```

### 2.11 数据集合

　　dict() 创建一个字典

　　set() 创建一个集合

　　frozenset() 创建一个冻结的集合,冻结的集合不能进行添加和删除操作

### 2.12 其他相关

　　len()  返回一个对象的元素个数

　　enumerate()  获取枚举对象

enumerate()  举例

```
lst = ['alex','wusir','taibai']
for i,k in enumerate(lst):
    print('这是序号',i)
    print('这是元素',k)
```

all()  可迭代对象中全部是True,结果才是True

```
lst = [1,2,3,4,True,0,False]

lst1 = [1,2,3,4,True]

print(all(lst))

print(all(lst1))

结果:

False

True
```

any()  可迭代对象中有一个是True,就是True　

```
lst = [1,2,3,4,True,0,False]

lst1 = [1,2,3,4,True]

print(any(lst))

print(any(lst1))

结果:

False

True
```

zip()  函数用于将可迭代的对象作为参数,将对象中对应的元素打包成一个个元祖,

然后返回由这些元祖组成的内容,如果各个迭代器的元素个数不一致,则按照长度最短的返回

```
lst1 = [1,2,3]

lst2 = ['a','b','c','d']

lst3 = (11,12,13,14,15)

for i in zip(lst1,lst2,lst3):

    print(i)

结果:

(1, 'a', 11)

(2, 'b', 12)

(3, 'c', 13)　
```

### 2.13 lambda

匿名函数,为了解决一些简单的需求而设计的一句话函数

```
def func(n):
    return n**n
print(func(4))
 
f = lambda x: x**x
print(f(4))
 
结果:
256
256
```

lambda表示的是匿名函数,不需要用def来声明,一句话就可以声明出一个函数

语法:

　　函数名 = lambda 参数:返回值

注意:

　　1.函数的参数可以有多个,多个参数之间用逗号隔开

　　2.匿名函数不管多复杂.只能写一行.且逻辑结束后直接返回数据

　　3.返回值和正常的函数一样,可以是任意数据类型,返回值的时候只能返回一个不能返回多个

匿名函数并不是说一定没有名字,这里前面的变量就是一个函数名,说他是匿名原因是我们通过

__name__查看的时候是没有名字的.统一都叫做lambda.在调用的时候没有什么特别之处

像正常的函数调用既可

### 2.14 sorted

排序函数

语法:sorted(iterable,key=None,reverse=False)

iterable :  可迭代对象

key: 排序规则(排序函数),在sorted内部会将可迭代对象中的每一个元素传递给这个函数的参数.根据函数运算的结果进行排序

reverse :是否是倒叙,True  倒叙  False 正序

```
lst = [1,3,2,5,4]
lst2 = sorted(lst)
print(lst)    #原列表不会改变
print(lst2)   #返回的新列表是经过排序的
 
 
lst3 = sorted(lst,reverse=True)
print(lst3)   #倒叙
 
结果:
[1, 3, 2, 5, 4]
[1, 2, 3, 4, 5]
[5, 4, 3, 2, 1]
```

字典使用sorted排序

```
dic = {1:'a',3:'c',2:'b'}
print(sorted(dic))   # 字典排序返回的就是排序后的key
 
结果:
[1,2,3]
```

和函数组合使用

```
# 定义一个列表,然后根据一元素的长度排序
lst = ['天龙八部','西游记','红楼梦','三国演义']
 
# 计算字符串的长度
def func(s):
    return len(s)
print(sorted(lst,key=func))
 
# 结果:
# ['西游记', '红楼梦', '天龙八部', '三国演义']
```

和lambda组合使用

```
lst = ['天龙八部','西游记','红楼梦','三国演义']
 
print(sorted(lst,key=lambda s:len(s)))
 
结果:
['西游记', '红楼梦', '天龙八部', '三国演义']
 
 
lst = [{'id':1,'name':'alex','age':18},
    {'id':2,'name':'wusir','age':17},
    {'id':3,'name':'taibai','age':16},]
 
# 按照年龄对学生信息进行排序
 
print(sorted(lst,key=lambda e:e['age']))
 
结果:
[{'id': 3, 'name': 'taibai', 'age': 16}, {'id': 2, 'name': 'wusir', 'age': 17}, {'id': 1, 'name': 'alex', 'age': 18}]
```

### 2.15 filter

筛选过滤

语法:  filter(function,iterable)

function: 用来筛选的函数,在filter中会自动的把iterable中的元素传递给function,然后根据function返回的True或者False来判断是否保留此项数据

iterable:可迭代对象

```
lst = [{'id':1,'name':'alex','age':18},
        {'id':1,'name':'wusir','age':17},
        {'id':1,'name':'taibai','age':16},]
 
ls = filter(lambda e:e['age'] > 16,lst)
 
print(list(ls))
 
结果:
[{'id': 1, 'name': 'alex', 'age': 18},
 {'id': 1, 'name': 'wusir', 'age': 17}]
```

### 2.16 map

映射函数

语法:  map(function,iterable) 可以对可迭代对象中的每一个元素进映射,分别取执行function

计算列表中每个元素的平方,返回新列表

```
lst = [1,2,3,4,5]

def func(s):

    return  s*s

mp = map(func,lst)

print(mp)

print(list(mp))
```

改写成lambda

```
lst = [1,2,3,4,5]

print(list(map(lambda s:s*s,lst)))
```

计算两个列表中相同位置的数据的和

```
lst1 = [1, 2, 3, 4, 5]

lst2 = [2, 4, 6, 8, 10]

print(list(map(lambda x, y: x+y, lst1, lst2)))

结果:

[3, 6, 9, 12, 15]
```

### 2.17 reduce

```
from functools import reduce
def func(x,y):
    return x + y

# reduce 的使用方式:
# reduce(函数名,可迭代对象)  # 这两个参数必须都要有,缺一个不行

ret = reduce(func,[3,4,5,6,7])
print(ret)  # 结果 25
reduce的作用是先把列表中的前俩个元素取出计算出一个值然后临时保存着,
接下来用这个临时保存的值和列表中第三个元素进行计算,求出一个新的值将最开始
临时保存的值覆盖掉,然后在用这个新的临时值和列表中第四个元素计算.依次类推

注意:我们放进去的可迭代对象没有更改
以上这个例子我们使用sum就可以完全的实现了.我现在有[1,2,3,4]想让列表中的数变成1234,就要用到reduce了.
普通函数版
from functools import reduce

def func(x,y):

    return x * 10 + y
    # 第一次的时候 x是1 y是2  x乘以10就是10,然后加上y也就是2最终结果是12然后临时存储起来了
    # 第二次的时候x是临时存储的值12 x乘以10就是 120 然后加上y也就是3最终结果是123临时存储起来了
    # 第三次的时候x是临时存储的值123 x乘以10就是 1230 然后加上y也就是4最终结果是1234然后返回了

l = reduce(func,[1,2,3,4])
print(l)


匿名函数版
l = reduce(lambda x,y:x*10+y,[1,2,3,4])
print(l)
```

在Python2.x版本中recude是直接 import就可以的, Python3.x版本中需要从functools这个包中导入

龟叔本打算将 lambda 和 reduce 都从全局名字空间都移除, 舆论说龟叔不喜欢lambda 和 reduce

最后lambda没删除是因为和一个人写信写了好多封,进行交流然后把lambda保住了.

参考资料:

<https://www.processon.com/view/link/5b4ee15be4b0edb750de96ac>