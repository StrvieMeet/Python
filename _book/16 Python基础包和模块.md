[TOC]



# Python 包和模块

## 一.包

你们听到的包,可不是女同胞疯狂喜欢的那个包,我们来看看这个是啥包
官方解释:

```
Packages are a way of structuring Python’s module namespace by using “dotted module names”
包是一种通过使用‘.模块名’来组织python模块名称空间的方式。

#具体的：包就是一个包含有__init__.py文件的文件夹，所以其实我们创建包的目的就是为了用文件夹将文件/模块组织起来

#需要强调的是：
　　1. 在python3中，即使包下没有__init__.py文件，import 包仍然不会报错，而在python2中，包下一定要有该文件，否则import 包报错

　　2. 创建包的目的不是为了运行，而是被导入使用，记住，包只是模块的一种形式而已，包的本质就是一种模块
```

为什么要使用包呢?
包的本质就是一个文件夹，那么文件夹唯一的功能就是将文件组织起来
随着功能越写越多，我们无法将所以功能都放到一个文件中，于是我们使用模块去组织功能，而随着模块越来越多，我们就需要用文件夹将模块文件组织起来，以此来提高程序的结构性和可维护性

注意事项

```
#1.关于包相关的导入语句也分为import和from ... import ...两种，但是无论哪种，无论在什么位置，在导入时都必须遵循一个原则：凡是在导入时带点的，点的左边都必须是一个包，否则非法。可以带有一连串的点，如item.subitem.subsubitem,但都必须遵循这个原则。但对于导入后，在使用时就没有这种限制了，点的左边可以是包,模块，函数，类(它们都可以用点的方式调用自己的属性)。

#2、import导入文件时，产生名称空间中的名字来源于文件，import 包，产生的名称空间的名字同样来源于文件，即包下的__init__.py，导入包本质就是在导入该文件

#3、包A和包B下有同名模块也不会冲突，如A.a与B.a来自俩个命名空间
```

### 1.1 包的使用

示例文件

```
glance/                   #Top-level package

├── __init__.py      #Initialize the glance package

├── api                  #Subpackage for api

│   ├── __init__.py

│   ├── policy.py

│   └── versions.py

├── cmd                #Subpackage for cmd

│   ├── __init__.py

│   └── manage.py

└── db                  #Subpackage for db

    ├── __init__.py

    └── models.py
    
```

### 1.2 文件内容

```
#policy.py
def get():
    print('from policy.py')

#versions.py
def create_resource(conf):
    print('from version.py: ',conf)

#manage.py
def main():
    print('from manage.py')

#models.py
def register_models(engine):
    print('from models.py: ',engine)

包所包含的文件内容
```

执行文件与示范文件在同级目录下

### 1.3 包的使用之import

```
1 import glance.db.models
2 glance.db.models.register_models('mysql') 
```

单独导入包名称时不会导入包中所有包含的所有子模块，如

```
#在与glance同级的test.py中
import glance
glance.cmd.manage.main()

'''
执行结果：
AttributeError: module 'glance' has no attribute 'cmd'

'''
```

解决方法：

```
1 #glance/__init__.py
2 from . import cmd
3 
4 #glance/cmd/__init__.py
5 from . import manage
```

执行：

```
1 #在于glance同级的test.py中
2 import glance
3 glance.cmd.manage.main()
```

### 1.4 包的使用之from ... import ...

需要注意的是from后import导入的模块，必须是明确的一个不能带点，否则会有语法错误，如：from a import b.c是错误语法

from glance.api import *

在讲模块时，我们已经讨论过了从一个模块内导入所有，此处我们研究从一个包导入所有。

此处是想从包api中导入所有，实际上该语句只会导入包api下__init**.py文件中定义的名字，我们可以在这个文件中定义_all**:

```
#在__init__.py中定义
x=10

def func():
    print('from api.__init.py')

__all__=['x','func','policy']
```

此时我们在于glance同级的文件中执行from glance.api import *就导入__all__中的内容（versions仍然不能导入）。

```
#在__init__.py中定义
x=10

def func():
    print('from api.__init.py')

__all__=['x','func','policy']
```

此时我们在于glance同级的文件中执行from glance.api import *就导入__all__中的内容（versions仍然不能导入）。

练习：

```
#执行文件中的使用效果如下，请处理好包的导入
from glance import *

get()
create_resource('a.conf')
main()
register_models('mysql')

#在glance.__init__.py中
from .api.policy import get
from .api.versions import create_resource

from .cmd.manage import main
from .db.models import  register_models

__all__=['get','create_resource','main','register_models']
```

### 1.5 绝对导入和相对导入

我们的最顶级包glance是写给别人用的，然后在glance包内部也会有彼此之间互相导入的需求，这时候就有绝对导入和相对导入两种方式：

绝对导入：以glance作为起始

相对导入：用.或者..的方式最为起始（只能在一个包中使用，不能用于不同目录内）

例如：我们在glance/api/version.py中想要导入glance/cmd/manage.py

```
1 在glance/api/version.py
2 
3 #绝对导入
4 from glance.cmd import manage
5 manage.main()
6 
7 #相对导入
8 from ..cmd import manage
9 manage.main()
```

测试结果：注意一定要在于glance同级的文件中测试

```
1 from glance.api import versions 
```

包以及包所包含的模块都是用来被导入的，而不是被直接执行的。而环境变量都是以执行文件为准的

比如我们想在glance/api/versions.py中导入glance/api/policy.py，有的同学一抽这俩模块是在同一个目录下，十分开心的就去做了，它直接这么做

```
1 #在version.py中
2 
3 import policy
4 policy.get()
```

没错，我们单独运行version.py是一点问题没有的，运行version.py的路径搜索就是从当前路径开始的，于是在导入policy时能在当前目录下找到

但是你想啊，你子包中的模块version.py极有可能是被一个glance包同一级别的其他文件导入，比如我们在于glance同级下的一个test.py文件中导入version.py，如下

```
 1 from glance.api import versions
 2 
 3 '''
 4 执行结果:
 5 ImportError: No module named 'policy'
 6 '''
 7 
 8 '''
 9 分析:
10 此时我们导入versions在versions.py中执行
11 import policy需要找从sys.path也就是从当前目录找policy.py,
12 这必然是找不到的
13 '''
```

绝对导入与相对导入总结

```
绝对导入与相对导入
# 绝对导入: 以执行文件的sys.path为起始点开始导入,称之为绝对导入
#        优点: 执行文件与被导入的模块中都可以使用
#        缺点: 所有导入都是以sys.path为起始点,导入麻烦

# 相对导入: 参照当前所在文件的文件夹为起始开始查找,称之为相对导入
#        符号: .代表当前所在文件的文件加,..代表上一级文件夹,...代表上一级的上一级文件夹
#        优点: 导入更加简单
#        缺点: 只能在导入包中的模块时才能使用
　　　　  #注意:
　　　　　　　　1. 相对导入只能用于包内部模块之间的相互导入,导入者与被导入者都必须存在于一个包内
　　　　　　　　2. attempted relative import beyond top-level package # 试图在顶级包之外使用相对导入是错误的,言外之意,必须在顶级包内使用相对导入,每增加一个.代表跳到上一级文件夹,而上一级不应该超出顶级包
```

## 二. random模块

```
>>> import random
#随机小数
>>> random.random()      # 大于0且小于1之间的小数
0.7664338663654585
>>> random.uniform(1,3) #大于1小于3的小数
1.6270147180533838
#恒富：发红包

#随机整数
>>> random.randint(1,5)  # 大于等于1且小于等于5之间的整数
>>> random.randrange(1,10,2) # 大于等于1且小于10之间的奇数


#随机选择一个返回
>>> random.choice([1,'23',[4,5]])  # #1或者23或者[4,5]
#随机选择多个返回，返回的个数为函数的第二个参数
>>> random.sample([1,'23',[4,5]],2) # #列表元素任意2个组合
[[4, 5], '23']


#打乱列表顺序
>>> item=[1,3,5,7,9]
>>> random.shuffle(item) # 打乱次序
>>> item
[5, 1, 3, 7, 9]
>>> random.shuffle(item)
>>> item
[5, 9, 7, 1, 3]
```

练习：生成随机验证码

```
import random

def v_code():

    code = ''
    for i in range(5):

        num=random.randint(0,9)
        alf=chr(random.randint(65,90))
        add=random.choice([num,alf])
        code="".join([code,str(add)])

    return code

print(v_code())
```

### 2.1 打印进度条

```
#=========知识储备==========
#进度条的效果
[#             ]
[##            ]
[###           ]
[####          ]

#指定宽度
print('[%-15s]' %'#')
print('[%-15s]' %'##')
print('[%-15s]' %'###')
print('[%-15s]' %'####')

#打印%
print('%s%%' %(100)) #第二个%号代表取消第一个%的特殊意义

#可传参来控制宽度
print('[%%-%ds]' %50) #[%-50s]
print(('[%%-%ds]' %50) %'#')
print(('[%%-%ds]' %50) %'##')
print(('[%%-%ds]' %50) %'###')


#=========实现打印进度条函数==========
import sys
import time

def progress(percent,width=50):
    if percent >= 1:
        percent=1
    show_str = ('%%-%ds' % width) % (int(width*percent)*'|')
    print('\r%s %d%%' %(show_str, int(100*percent)), end='')


#=========应用==========
data_size=1025
recv_size=0
while recv_size < data_size:
    time.sleep(0.1) #模拟数据的传输延迟
    recv_size+=1024 #每次收1024

    percent=recv_size/data_size #接收的比例
    progress(percent,width=70) #进度条的宽度70
```

## 三. shutil模块

高级的 文件、文件夹、压缩包 处理模块

shutil.copyfileobj(fsrc, fdst[, length])
将文件内容拷贝到另一个文件中

```
1 import shutil
2  
3 shutil.copyfileobj(open('old.xml','r'), open('new.xml', 'w'))
```

shutil.copyfile(src, dst)
拷贝文件

```
1 shutil.copyfile('f1.log', 'f2.log') #目标文件无需存在
```

shutil.copymode(src, dst)
仅拷贝权限。内容、组、用户均不变

```
1 shutil.copymode('f1.log', 'f2.log') #目标文件必须存在
```

shutil.copystat(src, dst)
仅拷贝状态的信息，包括：mode bits, atime, mtime, flags

```
1 shutil.copystat('f1.log', 'f2.log') #目标文件必须存在
```

shutil.copy(src, dst)
拷贝文件和权限

```
1 import shutil
2  
3 shutil.copy('f1.log', 'f2.log')
shutil.copy2(src, dst)
```

拷贝文件和状态信息

```
1 import shutil
2  
3 shutil.copy2('f1.log', 'f2.log')
shutil.ignore_patterns(*patterns)
shutil.copytree(src, dst, symlinks=False, ignore=None)
```

递归的去拷贝文件夹

```
1 import shutil
2  
3 shutil.copytree('folder1', 'folder2', ignore=shutil.ignore_patterns('*.pyc', 'tmp*')) #目标目录不能存在，注意对folder2目录父级目录要有可写权限，ignore的意思是排除 
```

拷贝软连接

```
import shutil

shutil.copytree('f1', 'f2', symlinks=True, ignore=shutil.ignore_patterns('*.pyc', 'tmp*'))

'''
通常的拷贝都把软连接拷贝成硬链接，即对待软连接来说，创建新的文件
'''
```

shutil.rmtree(path[, ignore_errors[, onerror]])
递归的去删除文件

```
1 import shutil
2  
3 shutil.rmtree('folder1')
shutil.move(src, dst)
```

递归的去移动文件，它类似mv命令，其实就是重命名。

```
1 import shutil
2  
3 shutil.move('folder1', 'folder3')
shutil.make_archive(base_name, format,...)
```

创建压缩包并返回文件路径，例如：zip、tar

```
base_name： 压缩包的文件名，也可以是压缩包的路径。只是文件名时，则保存至当前目录，否则保存至指定路径，
如 data_bak                       =>保存至当前路径
如：/tmp/data_bak =>保存至/tmp/
format： 压缩包种类，“zip”, “tar”, “bztar”，“gztar”
root_dir：   要压缩的文件夹路径（默认当前目录）
owner：  用户，默认当前用户
group：  组，默认当前组
logger： 用于记录日志，通常是logging.Logger对象

#将 /data 下的文件打包放置当前程序目录
import shutil
ret = shutil.make_archive("data_bak", 'gztar', root_dir='/data')
  
  
#将 /data下的文件打包放置 /tmp/目录
import shutil
ret = shutil.make_archive("/tmp/data_bak", 'gztar', root_dir='/data')
```

### 3.1 压缩包

shutil 对压缩包的处理是调用 ZipFile 和 TarFile 两个模块来进行的，详细：
zipfile压缩解压缩

```
import zipfile

# 压缩
z = zipfile.ZipFile('laxi.zip', 'w')
z.write('a.log')
z.write('data.data')
z.close()

# 解压
z = zipfile.ZipFile('laxi.zip', 'r')
z.extractall(path='.')
z.close()
```

tarfile压缩解压缩

```
import tarfile

# 压缩
>>> t=tarfile.open('/tmp/egon.tar','w')
>>> t.add('/test1/a.py',arcname='a.bak')
>>> t.add('/test1/b.py',arcname='b.bak')
>>> t.close()


# 解压
>>> t=tarfile.open('/tmp/egon.tar','r')
>>> t.extractall('/egon')
>>> t.close()
```