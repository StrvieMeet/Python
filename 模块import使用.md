### **一. import**

#### **1.1 import 使用**

import 翻译过来是一个导入的意思。

这里一定要给同学强调哪个文件执行文件，和哪个文件是被执行模块。   

​     模块可以包含可执行的语句和函数的定义，这些语句的目的是初始化模块，它们只在模块名第一次遇到导入import语句时才执行（import语句是可以在程序中的任意位置使用的,且针对同一个模块import很多次,为了防止你重复导入，python的优化手段是：第一次导入后就将模块名加载到内存了，后续的import语句仅是对已经加载到内存中的模块对象增加了一次引用，不会重新执行模块内的语句），如下 import meet 只在第一次导入时才执行meet.py内代码,此处的显式效果是只打印一次'from the meet.py',当然其他的顶级代码也都被执行了,只不过没有显示效果.

```python
import meet
import meet
import meet
import meet
import meet

执行结果：只是打印一次：
from the meet.py
```

**重复导入会直接引用内存中已经加载好的结果**

#### 1.2 第一次导入模块执行三件事

​        1.创建一个以模块名命名的名称空间。

​        2.执行这个名称空间（即导入的模块）里面的代码。

​        3.通过此模块名. 的方式引用该模块里面的内容（变量，函数名，类名等）。 这个名字和变量名没什么区别，都是‘第一类的’，且使用meet名字的方式可以访问meet.py文件中定义的名字，meet.名字与test.py中的名字来自两个完全不同的地方。

#### **1.3 被导入模块有独立的名称空间**

​    每个模块都是一个独立的名称空间，定义在这个模块中的函数，把这个模块的名称空间当做全局名称空间，这样我们在编写自己的模块时，就不用担心我们定义在自己模块中全局变量会在被导入时，与使用者的全局变量冲突。

示例：

```python
当前是test.py

import meet
name = 'alex'
print(name)
print(meet.name)

'''
运行结果:
from the meet.py
alex
郭宝元
'''

def read1():
    print(666)
meet.read1()

'''
运行结果:
from the meet.py
meet模块： 郭宝元
'''

name = '日天'
meet.change()
print(name)
print(meet.name)

'''
运行结果:
from the meet.py
日天
宝浪
'''
```

**让同学们将上面的代码练习一下。**

#### **1.4 为模块起别名**

别名其实就是一个外号,我们小的时候，都喜欢给学生们起外号对吧。

**1. 好处可以将很长的模块名改成很短,方便使用.**

```
import tbjx as t
t.read1()
```

**2. 有利于代码的扩展和优化。**

```python
#mysql.py
def sqlparse():
    print('from mysql sqlparse')
#oracle.py
def sqlparse():
    print('from oracle sqlparse')
```

```python
#test.py
db_type=input('>>: ')
if db_type == 'mysql':
    import mysql as db
elif db_type == 'oracle':
    import oracle as db

db.sqlparse()
```

#### **1.5 导入多个模块**

​    我们以后再开发过程中，免不了会在一个文件中，导入多个模块，推荐写法是一个一个导入。

```
import os,sys,json   # 这样写可以但是不推荐

推荐写法

import os
import sys
import json
```

**多行导入：易于阅读 易于编辑 易于搜索 易于维护。**

