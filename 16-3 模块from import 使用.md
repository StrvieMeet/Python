### **一 from ... import ...**

#### **1.1 from ... import ... 使用**

```
from ... import ... 的使用示例。

from meet import name, read1
print(name)
read1()
'''
执行结果：
from the meet.py
太白金星
meet模块： 郭宝元
'''
```

#### **1.2 from...import... 与import对比**

​    唯一的区别就是：使用from...import...则是将spam中的名字直接导入到当前的名称空间中，所以在当前名称空间中，直接使用名字就可以了、无需加前缀：tbjx.

from...import...的方式有好处也有坏处

​    好处：使用起来方便了

​    坏处：容易与当前执行文件中的名字冲突

示例演示：

1. **执行文件有与模块同名的变量或者函数名，会有覆盖效果。**

```python
name = 'oldboy'
from meet import name, read1, read2
print(name)  
'''
执行结果：
郭宝元
'''
----------------------------------------
from meet import name, read1, read2
name = 'oldboy'
print(name)  

'''
执行结果：
oldboy
'''
----------------------------------------
def read1():
    print(666)
from meet import name, read1, read2
read1()

'''
执行结果：
meet模块： 郭宝元
'''
----------------------------------------

from meet import name, read1, read2
def read1():
    print(666)
read1()

'''
执行结果：
meet模块： 666
'''

```

**2. 当前位置直接使用read1和read2就好了，执行时，仍然以meet.py文件全局名称空间**

```
#测试一：导入的函数read1，执行时仍然回到meet.py中寻找全局变量 'alex'
#test.py
from meet import read1
name = 'alex'
read1()
'''
执行结果:
from the meet.py
meet->read1->name = '郭宝元'
'''

#测试二:导入的函数read2，执行时需要调用read1(),仍然回到meet.py中找
#read1()

#test.py
from meet import read2
def read1():
    print('==========')
read2()

'''
执行结果:
from the meet.py
meet模块
meet模块： 郭宝元
'''

```

#### 1.3 from … import也支持as

通过这种方式引用模块也可以对模块进行改名。

```python
from meet import read1 as read
read()

```

#### **1.4 一行导入多个**

```
from tbjx import read1,read2,name
```

#### 1.5 from ... import \*

​    from meet import *  把meet中所有的不是以下划线(_)开头的名字都导入到当前位置

​    大部分情况下我们的python程序不应该使用这种导入方式，因为*你不知道你导入什么名字，很有可能会覆盖掉你之前已经定义的名字。而且可读性极其的差，在交互式环境中导入时没有问题。

可以使用**all**来控制*（用来发布新版本），在meet.py中新增一行

```
__all__=['name','read1'] #这样在另外一个文件中用from spam import *就这能导入列表中规定的两个名字
```

#### **1.6 模块循环导入问题**

​    模块循环/嵌套导入抛出异常的根本原因是由于在python中模块被导入一次之后，就不会重新导入，只会在第一次导入时执行模块内代码

​    在我们的项目中应该尽量避免出现循环/嵌套导入，如果出现多个模块都需要共享的数据，可以将共享的数据集中存放到某一个地方在程序出现了循环/嵌套导入后的异常分析、解决方法如下（**了解，以后尽量避免**）

示范文件内容如下

```python
#创建一个m1.py
print('正在导入m1')
from m2 import y
x='m1

#创建一个m2.py
print('正在导入m2')
from m1 import x
y='m2'

#创建一个run.py
import m1

#测试一
执行run.py会抛出异常
正在导入m1
正在导入m2
Traceback (most recent call last):
  File "/python项目/run.py", line 1, in <module>
    import m1
  File "/python项目/m1.py", line 2, in <module>
    from m2 import y
  File "/python项目/m2.py", line 2, in <module>
    from m1 import x
ImportError: cannot import name 'x'

#测试一结果分析
先执行run.py--->执行import m1，开始导入m1并运行其内部代码--->打印内容"正在导入m1"
--->执行from m2 import y 开始导入m2并运行其内部代码--->打印内容“正在导入m2”--->执行from m1 import x,由于m1已经被导入过了，所以不会重新导入，所以直接去m1中拿x，然而x此时并没有存在于m1中，所以报错

#测试二:执行文件不等于导入文件，比如执行m1.py不等于导入了m1
正在导入m1
正在导入m2
Traceback (most recent call last):
正在导入m1
  File "/python项目/m1.py", line 2, in <module>
    from m2 import y
  File "/python项目/m2.py", line 2, in <module>
    from m1 import x
  File "/python项目/m1.py", line 2, in <module>
    from m2 import y
ImportError: cannot import name 'y'


#测试二分析
执行m1.py，打印“正在导入m1”，执行from m2 import y ，导入m2进而执行m2.py内部代码--->打印"正在导入m2"，执行from m1 import x，此时m1是第一次被导入，执行m1.py并不等于导入了m1，于是开始导入m1并执行其内部代码--->打印"正在导入m1"，执行from m1 import y，由于m1已经被导入过了，所以无需继续导入而直接问m2要y，然而y此时并没有存在于m2中所以报错


# 解决方法:
方法一:导入语句放到最后
#m1.py
print('正在导入m1')

x='m1'

from m2 import y

#m2.py
print('正在导入m2')
y='m2'

from m1 import x

方法二:导入语句放到函数中
#m1.py
print('正在导入m1')

def f1():
    from m2 import y
    print(x,y)

x = 'm1'

# f1()
#m2.py
print('正在导入m2')

def f2():
    from m1 import x
    print(x,y)

y = 'm2'

#run.py
import m1

m1.f1()
```

### 