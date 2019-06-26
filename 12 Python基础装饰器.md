# Python 装饰器

## 一.装饰器

在讲解装饰器之前的时候我们先讲解一下开放封闭原则

### **1. 开放封闭原则**

​    什么是开放封闭原则？有的同学问开放，封闭这是两个反义词这还能组成一个原则么？这不前后矛盾么？其实不矛盾。开放封闭原则是分情况讨论的。

​    我们的软件一旦上线之后（比如你的软件主要是多个函数组成的）,那么这个软件对功能的扩展应该是开放的，比如你的游戏一直在迭代更新，推出新的玩法，新功能。但是对于源代码的修改是封闭的。你就拿函数举例，如果你的游戏源代码中有一个函数是闪躲的功能，那么你这个函数肯定是被多个地方调用的，比如对方扔手雷，对方开枪，对方用刀，你都会调用你的闪躲功能，那么如果你的闪躲功能源码进行改变了，或者调用方式改变了，当对方发起相应的动作，你在调用你的闪躲功能，就会发生问题。所以，开放封闭原则具体定义是这样：

​    1.对扩展是开放的

​        我们说，任何一个程序，不可能在设计之初就已经想好了所有的功能并且未来不做任何更新和修改。所以我们必须允许代码扩展、添加新功能。

​    2.对修改是封闭的

​        就像我们刚刚提到的，因为我们写的一个函数，很有可能已经交付给其他人使用了，如果这个时候我们对函数内部进行修改，或者修改了函数的调用方式，很有可能影响其他已经在使用该函数的用户。OK，理解了开封封闭原则之后，我们聊聊装饰器。

​    什么是装饰器？从字面意思来分析，先说装饰，什么是装饰? 装饰就是添加新的，

​    比如我现在不会飞，怎么才能让我会飞？给我额外增加一个翅膀，我就能飞了。那么你给我加一个翅膀，它会改变我原来的行为么？我之前的吃喝拉撒睡等生活方式都不会改变。它就是在我原来的基础上，添加了一个新的功能。

今天我们讲的装饰器（翅膀）是以功能为导向的，就是一个函数。

被装饰的对象：我本人，其实也是一个函数。

**所以装饰器最终最完美的定义就是：在不改变原被装饰的函数的源代码以及调用方式下，为其添加额外的功能。**

### **2. 初识装饰器**

接下来，我们通过一个例子来为大家讲解这个装饰器：

需求介绍：你现在xx科技有限公司的开发部分任职，领导给你一个业务需求让你完成：让你写代码测试小明同学写的函数的执行效率。

```
def index():
    print('欢迎访问博客园主页')
```

**版本1：**

​    需求分析：你要想测试此函数的执行效率，你应该怎么做？应该在此函数执行前记录一个时间， 执行完毕之后记录一个时间，这个时间差就是具体此函数的执行效率。那么执行时间如何获取呢？ 可以利用time模块，有一个time.time()功能。

```
import time
print(time.time())
```

​    此方法返回的是格林尼治时间，是此时此刻距离1970年1月1日0点0分0秒的时间秒数。也叫时间戳，他是一直变化的。所以要是计算index的执行效率就是在执行前后计算这个时间戳的时间，然后求差值即可。

```python
import time
def index():
    print('欢迎访问博客园主页')

start_time = time.time()
index()
end_time = time.time()
print(f'此函数的执行效率为{end_time-start_time}')
```

由于index函数只有一行代码，执行效率太快了，所以我们利用time模块的一个sleep模拟一下

```python
import time
def index():
    time.sleep(2)  # 模拟一下网络延迟以及代码的效率
    print('欢迎访问博客园主页')

start_time = time.time()
index()
end_time = time.time()
print(f'此函数的执行效率为{end_time-start_time}')
```

**版本1分析**：你现在已经完成了这个需求，但是有什么问题没有？ 虽然你只写了四行代码，但是你完成的是一个测试其他函数的执行效率的功能，如果让你测试一下，小张，小李，小刘的函数效率呢？ 你是不是全得复制：

```python
import time
def index():
    time.sleep(2)  # 模拟一下网络延迟以及代码的效率
    print('欢迎访问博客园首页')

def home(name):
    time.sleep(3)  # 模拟一下网络延迟以及代码的效率
    print(f'欢迎访问{name}主页')

start_time = time.time()
index()
end_time = time.time()
print(f'此函数的执行效率为{end_time-start_time}')

start_time = time.time()
home('太白')
end_time = time.time()
print(f'此函数的执行效率为{end_time-start_time}')
```

重复代码太多了，所以要想解决重复代码的问题，怎么做？我们是不是学过函数，函数就是以功能为导向，减少重复代码，好我们继续整改。

**版本2：**

```python
import time

def index():
    time.sleep(2)  # 模拟一下网络延迟以及代码的效率
    print('欢迎访问博客园主页')

def inner():
    start_time = time.time()
    index()
    end_time = time.time()
    print(f'此函数的执行效率为{end_time-start_time}')

inner()
```

但是你这样写也是有问题的，你虽然将测试功能的代码封装成了一个函数，但是这样，你只能测试小明同学的的函数index，你要是测试其他同事的函数呢？你怎么做？

```python
import time
def index():
    time.sleep(2)  # 模拟一下网络延迟以及代码的效率
    print('欢迎访问博客园主页')

def home(name):
    time.sleep(3)  # 模拟一下网络延迟以及代码的效率
    print(f'欢迎访问{name}主页')

def inner():
    start_time = time.time()
    index()
    home('太白')
    end_time = time.time()
    print(f'此函数的执行效率为{end_time-start_time}')

timer()
```

你要是像上面那么做，每次测试其他同事的代码还需要手动改，这样是不是太low了？所以如何变成动态测试其他函数？我们是不是学过函数的传参？能否将被装饰函数的函数名作为函数的参数传递进去呢？

 **版本3：**

```python
import time
def index():
    time.sleep(2)  # 模拟一下网络延迟以及代码的效率
    print('欢迎访问博客园主页')

def home(name):
    time.sleep(3)  # 模拟一下网络延迟以及代码的效率
    print(f'欢迎访问{name}主页')

def timmer(func):  # func == index 函数
    start_time = time.time()
    func()  # index()
    end_time = time.time()
    print(f'此函数的执行效率为{end_time-start_time}')

timmer(index)
```

这样我将index函数的函数名作为参数传递给timmer函数，然后在timmer函数里面执行index函数，这样就变成动态传参了。好，你们现在将版本3的代码快速练一遍。 大家练习完了之后，发现有什么问题么？ 对比着开放封闭原则说： 首先，index函数除了完成了自己之前的功能，还增加了一个测试执行效率的功能，对不？所以也符合开放原则。 其次，index函数源码改变了么？没有，但是执行方式改变了，所以不符合封闭原则。 原来如何执行？ index() 现在如何执行？ inner(index),这样会造成什么问题？ 假如index在你的项目中被100处调用，那么这相应的100处调用我都得改成inner(index)。 非常麻烦，也不符合开放封闭原则。

**版本4：**实现真正的开放封闭原则：装饰器。

这个也很简单，就是我们昨天讲过的闭包，只要你把那个闭包的执行过程整清楚，那么这个你想不会都难。

```python
import time
def index():
    time.sleep(2)  # 模拟一下网络延迟以及代码的效率
    print('欢迎访问博客园主页')

def home(name):
    time.sleep(3)  # 模拟一下网络延迟以及代码的效率
    print(f'欢迎访问{name}主页')
```

你将上面的inner函数在套一层最外面的函数timer，然后将里面的inner函数名作为最外面的函数的返回值，这样简单的装饰器就写好了，一点新知识都没有加，这个如果不会就得多抄几遍，抄的时候要理解一下代码。

```python
def timer(func):  # func = index
    def inner():
        start_time = time.time()
        func()
        end_time = time.time()
        print(f'此函数的执行效率为{end_time-start_time}')
    return inner
# f = timer(index)
# f()
```

我们分析一下，代码，代码执行到这一行：f = timer(index) 先执行谁？看见一个等号先要执行等号右边， timer(index) 执行timer函数将index函数名传给了func形参。内层函数inner执行么？不执行，inner函数返回 给f变量。所以我们执行f() 就相当于执行inner闭包函数。 f(),这样既测试效率又执行了原函数，有没有问题？当然有啦！！版本4你要解决原函数执行方式不改变的问题，怎么做？ 所以你可以把 f 换成 index变量就完美了！ index = timer(index) index()带着同学们将这个流程在执行一遍，特别要注意 函数外面的index实际是inner函数的内存地址而不是index函数。让学生们抄一遍，理解一下，这个timer就是最简单版本装饰器，在不改变原index函数的源码以及调用方式前提下，为其增加了额外的功能，测试执行效率。

### **3. 带返回值的装饰器**

​    你现在这个代码，完成了最初版的装饰器，但是还是不够完善，因为你被装饰的函数index可能会有返回值，如果有返回值，你的装饰器也应该不影响，开放封闭原则嘛。但是你现在设置一下试试：

```python
import time
def index():
    time.sleep(2)  # 模拟一下网络延迟以及代码的效率
    print('欢迎访问博客园主页')
    return '访问成功'

def timer(func):  # func = index
    def inner():
        start_time = time.time()
        func()
        end_time = time.time()
        print(f'此函数的执行效率为{end_time-start_time}')
    return inner

index = timer(index)
print(index())  # None
```

加上装饰器之后，他的返回值为None，为什么？因为你现在的index不是函数名index，这index实际是inner函数名。所以index() 等同于inner() 你的 '访问成功'返回值应该返回给谁？应该返回给index，这样才做到开放封闭，实际返回给了谁？实际返回给了func，所以你要更改一下你的装饰器代码，让其返回给外面的index函数名。 所以：你应该这么做：

```python
def timer(func):  # func = index
    def inner():
        start_time = time.time()
        ret = func()
        end_time = time.time()
        print(f'此函数的执行效率为{end_time-start_time}')
        return ret
    return inner

index = timer(index)  # inner
print(index())  # print(inner())
```

借助于内层函数inner，你将func的返回值，返回给了inner函数的调用者也就是函数外面的index，这样就实现了开放封闭原则，index返回值，确实返回给了'index'。

让同学们；练习一下。

### 4. 被装饰函数带参数的装饰器

到目前为止，你的被装饰函数还是没有传参呢？按照我们的开放封闭原则，加不加装饰器都不能影响你被装饰函数的使用。所以我们看一下。

```python
import time
def index():
    time.sleep(2)  # 模拟一下网络延迟以及代码的效率
    print('欢迎访问博客园主页')
    return '访问成功'

def home(name):
    time.sleep(3)  # 模拟一下网络延迟以及代码的效率
    print(f'欢迎访问{name}主页')

def timer(func):  # func = index
    def inner():
        start_time = time.time()
        func()
        end_time = time.time()
        print(f'此函数的执行效率为{end_time-start_time}')
    return inner

# 要想timer装饰home函数怎么做？
home = timer(home)
home('太白')
```

上面那么做，显然报错了，为什么？ 你的home这个变量是谁？是inner，home('太白')实际是inner('太白')但是你的'太白'这个实参应该传给谁？ 应该传给home函数，实际传给了谁？实际传给了inner，所以我们要通过更改装饰器的代码，让其将实参'太白'传给home.

```python
import time
def index():
    time.sleep(2)  # 模拟一下网络延迟以及代码的效率
    print('欢迎访问博客园主页')
    return '访问成功'
​
def home(name):
    time.sleep(3)  # 模拟一下网络延迟以及代码的效率
    print(f'欢迎访问{name}主页')
​
def timer(func):  # func = home
    def inner(name):
        start_time = time.time()
        func(name)  # home(name) == home('太白')
        end_time = time.time()
        print(f'此函数的执行效率为{end_time-start_time}')
    return inner
​
# 要想timer装饰home函数怎么做？
home = timer(home)
home('太白')
```

这样你就实现了，还有一个小小的问题，现在被装饰函数的形参只是有一个形参，如果要是多个怎么办？有人说多少个我就写多少个不就行了，那不行呀，你这个装饰器可以装饰N多个不同的函数，这些函数的参数是不统一的。所以你要有一种可以接受不定数参数的形参接受他们。这样，你就要想到*args，**kwargs。

```python
import time
def index():
    time.sleep(2)  # 模拟一下网络延迟以及代码的效率
    print('欢迎访问博客园主页')
    return '访问成功'

def home(name,age):
    time.sleep(3)  # 模拟一下网络延迟以及代码的效率
    print(name,age)
    print(f'欢迎访问{name}主页')

def timer(func):  # func = home
    def inner(*args,**kwargs):  # 函数定义时，*代表聚合：所以你的args = ('太白',18)
        start_time = time.time()
        func(*args,**kwargs)  # 函数的执行时，*代表打散：所以*args --> *('太白',18)--> func('太白',18)
        end_time = time.time()
        print(f'此函数的执行效率为{end_time-start_time}')
    return inner

home = timer(home)
home('太白',18)
```

这样利用*的打散与聚合的原理，将这些实参通过inner函数的中间完美的传递到给了相应的形参。

好将上面的代码在敲一遍。

### **5. 标准版装饰器**

代码优化：语法糖

根据我的学习，我们知道了，如果想要各给一个函数加一个装饰器应该是这样：

```python
def home(name,age):
    time.sleep(3)  # 模拟一下网络延迟以及代码的效率
    print(name,age)
    print(f'欢迎访问{name}主页')

def timer(func):  # func = home
    def inner(*args,**kwargs):
        start_time = time.time()
        func(*args,**kwargs)
        end_time = time.time()
        print(f'此函数的执行效率为{end_time-start_time}')
    return inner

home = timer(home)
home('太白',18)
```

 如果你想给home加上装饰器，每次执行home之前你要写上一句：home = timer(home)这样你在执行home函数 home('太白',18) 才是真生的添加了额外的功能。但是每次写这一句也是很麻烦。所以，Python给我们提供了一个简化机制，用一个很简单的符号去代替这一句话。

```python
def timer(func):  # func = home
    def inner(*args,**kwargs):
        start_time = time.time()
        func(*args,**kwargs)
        end_time = time.time()
        print(f'此函数的执行效率为{end_time-start_time}')
    return inner
​
@timer  # home = timer(home)
def home(name,age):
    time.sleep(3)  # 模拟一下网络延迟以及代码的效率
    print(name,age)
    print(f'欢迎访问{name}主页')
​
home('太白',18)
```

你看此时我调整了一下位置，你要是不把装饰器放在上面，timer是找不到的。home函数如果想要加上装饰器那么你就在home函数上面加上@home，就等同于那句话 home = timer(home)。这么做没有什么特殊意义，就是让其更简单化，比如你在影视片中见过野战军的作战时由于不方便说话，用一些简单的手势代表一些话语，就是这个意思。

**至此标准版的装饰器就是这个样子：**

```
def wrapper(func):
    def inner(*args,**kwargs):
        '''执行被装饰函数之前的操作'''
        ret = func
        '''执行被装饰函数之后的操作'''
        return ret
    return inner
```

这个就是标准的装饰器，完全符合代码开放封闭原则。这几行代码一定要背过，会用。

**此时我们要利用这个装饰器完成一个需求：简单版模拟博客园登录。** 此时带着学生们看一下博客园，说一下需求： 博客园登陆之后有几个页面，diary，comment，home，如果我要访问这几个页面，必须验证我是否已登录。 如果已经成功登录，那么这几个页面我都可以无阻力访问。如果没有登录，任何一个页面都不可以访问，我必须先登录，登录成功之后，才可以访问这个页面。我们用成功执行函数模拟作为成功访问这个页面，现在写三个函数，写一个装饰器，实现上述功能。

```python
login_status = {
    'username': None,
    'status': False,
}

def auth(func):
    def inner(*args,**kwargs):
        if login_status['status']:
            ret = func()
            return ret
        username = input('请输入用户名：').strip()
        password = input('请输入密码：').strip()
        if username == '太白' and password == '123':
            login_status['status'] = True
            ret = func()
            return ret
    return inner

@auth
def diary():
    print('欢迎访问日记页面')

@auth
def comment():
    print('欢迎访问评论页面')

@auth
def home():
    print('欢迎访问博客园主页')

diary()
comment()
home()
```

