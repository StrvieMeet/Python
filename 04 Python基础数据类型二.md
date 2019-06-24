# Python基础数据类型二

## 一.列表

列表是python的基础数据类型之一 ,其他编程语言也有类似的数据类型.

比如JS中的数 组, java中的数组等等. 它是以[ ]括起来, 每个元素用' , '隔开而且可以存放各种数据类型: 

列表是python中的基础数据类型之一，其他语言中也有类似于列表的数据类型，比如js中叫数组，他是以[]括起来，每个元素以逗号隔开，而且他里面可以存放各种数据类型比如：

```PYTHON
li = [‘alex’,123,Ture,(1,2,3,’wusir’),[1,2,3,’小明’,],{‘name’:’alex’}]
```

列表相比于字符串，不仅可以储存不同的数据类型，而且可以储存大量数据，

32位python的限制是 536870912 个元素,

64位python的限制是 1152921504606846975 个元素。而且列表是有序的，有索引值，可切片，方便取值。

```PYTHON
lst = [1,'a',True,[2,3,4]]
```

### 1.1 列表的索引

列表和字符串一样也拥有索引:

```PYTHON
lst = ['刘德华','周润发','周杰伦','向华强']
print(lst[0])  # 列表中第一个元素
print(lst[1])  # 列表中第二个元素
print(lst[2])  # 列表中第三个元素
```

注意:列表是可以进行修改的,这里和字符串不一样

```PYTHON
lst[3] = '王健林'
print(lst)
```

字符串修改

```PYTHON
s = '王思聪'
s[0] = '李'
print(s)
结果:
Traceback (most recent call last):
  File "D:/python_object/path2/test.py", line 1076, in <module>
    s[0] = '李'
TypeError: 'str' object does not support item assignment
```

### 1.2 列表的切片

```PYTHON
lst = ["麻花藤", "王剑林", "马芸", "周鸿医", "向华强"] 
print(lst[0:3])     # ['麻花藤', '王剑林', '马芸'] 
print(lst[:3])      # ['麻花藤', '王剑林', '马芸']
print(lst[1::2])    # ['王剑林', '周鸿医'] 也有步长 
print(lst[2::-1])   # ['马芸', '王剑林', '麻花藤'] 也可以倒着取 
print(lst[-1:-3:-2])    # 倒着带步长
```

**练习**

```PYTHON
li = [1, 3, 2, "a", 4, "b", 5,"c"]
通过对li列表的切片形成新的列表l1,l1 = [1,3,2]
通过对li列表的切片形成新的列表l2,l2 = ["a",4,"b"]
通过对li列表的切片形成新的列表l3,l3 = ["1,2,4,5]
通过对li列表的切片形成新的列表l4,l4 = [3,"a","b"]
通过对li列表的切片形成新的列表l5,l5 = ["c"]
通过对li列表的切片形成新的列表l6,l6 = ["b","a",3]
```

### 1.3 列表的增删改查

#### 1.3.1 增 

注意  list和str是不一样的. lst可以发生改变. 所以直接就在原来的对象上进行了操作

追加模式

```PYTHON
lst = ["麻花藤", "林俊杰", "周润发", "周芷若"] 
print(lst) 
lst.append("wusir") 
print(lst)
```

**练习**

输入用户信息,添加到列表中

```PYTHON
lst = []
while True:
    content = input("请输入你要录入的员工信息, 输入Q退出:")
    if content.upper() == 'Q':
        break
lst.append(content)
print(lst)
```

插入模式

```PYTHON
lst = ["麻花藤", "张德忠", "孔德福"]
lst.insert(1, "刘德华")    # 在1的位置插入刘德华. 原来的元素向后移动一位
print(lst)
```

迭代添加

```PYTHON
# 迭代添加
lst = ["王志文", "张一山", "苦海无涯"]
lst.extend(["麻花藤", "麻花不疼"])
print(lst)
```

#### 1.3.2 删除

```PYTHON
pop 通过下标删除元素(默认删除最后一个)

lst = ["麻花藤", "王剑林林", "李李嘉诚", "王富贵"] 
print(lst)
lst.pop()

deleted = lst.pop()
print('被删除的',deleted)
print(lst)

el = lst.pop(2)  # 删除下标位2的元素
print(el)        # 被删除的元素
print(lst)
```

remove　通过元素删除

```PYTHON
lst = ["麻花藤", "王剑林", "李嘉诚", "王富贵"]
lst.remove('王剑林')
print(lst)

结果:
['麻花藤', '李嘉诚', '王富贵']

lst.remove('哈哈')   # 删除不存在的元素
结果:
Traceback (most recent call last):
  File "D:/python_object/path2/test.py", line 1115, in <module>
    lst.remove('哈哈')   # 删除不存在的元素
ValueError: list.remove(x): x not in list
```

clear  清空

```PYTHON
lst = ["麻花藤", "王剑林", "李嘉诚", "王富贵"]
lst.clear()
print(lst)

结果:
[]
```

**练习**

```PYTHON
写代码，有如下列表，按照要求实现每一个功能
li = ["alex", "WuSir", "ritian", "barry", "wenzhou"]
请删除列表中的元素"ritian",并输出添加后的列表
请删除列表中的第2个元素，并输出删除的元素和删除元素后的列表
请删除列表中的第2至4个元素，并输出删除元素后的列表
```

#### 1.3.3 修改

索引切片修改

```PYTHON
# 修改 
lst = ["太白", "太黑", "五色", "银王", "⽇天"] 
lst[1] = "太污"   # 把1号元素修改成太污 print(lst) 
lst[1:4:3] = ["麻花藤", "哇靠"]     # 切片修改也OK. 如果步长不是1, 要注意. 元素的个 数 
print(lst) 
lst[1:4] = ["我是哪个村的村长王富贵"]  # 如果切片没有步长或者步长是1. 则不用关心个数 
print(lst)
```

#### 1.3.4 查询　　

```PYTHON
列表是一个可迭代对象,所以可以进行for循环

lst = ["麻花藤", "王剑林", "李嘉诚", "王富贵"]

for i in lst:
    print(i)

结果:
麻花藤
王剑林
李嘉诚
王富贵
```

**练习**

```PYTHON
li = ["alex", "WuSir", "ritian", "barry", "wenzhou"]
将列表li中第三个元素修改成'taibai'
将列表li中第四个元素修改成'女神'
将列表li中前三个元素修改成'alex1,alex2,alex3'
```

### 1.4 列表其他操作

#### 1.4.1反转

```PYTHON
li = ["alex", "WuSir", "ritian", "barry", "wenzhou"]

li.reverse()  # 把这个列表进行调转
print(li)
```

#### 1.4.2排序　　

```PYTHON
li = [1,2,3,4,84,5,2,8,2,11,88,2]
li.sort()              # 升序 排序
print(li)

li.sort(reverse=True)  # 降序 排序
print(li)
```

#### 1.4.3 统计

```PYTHON
li = [1,2,3,4,84,5,2,8,2,11,88,2]
num = li.count(3)   # 统计元素3出现的次数,和字符串中功能一样
print(num)
```

#### 1.4.4 通过元素获取下标

```PYTHON
li = [1,2,3,4,84,5,2,8,2,11,88,2]
n = li.index(5)
print(n)
```

#### 1.4.5 获取长度

```PYTHON
li = [1,2,3,4,84,5,2,8,2,11,88,2]
print(len(li))

结果:
12　
```

**练习**　　

```PYTHON
li = [1,3,6,9,2,4,6,8]

1.将以上这个列表中进行反转
2.将以上这个列表中进行降序排列

li = ["alex", "wusir", "taibai"]
利用下划线将列表的每一个元素拼接成字符串"alex_wusir_taibai"　　
```

### 1.5 列表的嵌套

注意:采用降维操作,一层一层的看就好

```PYTHON
lst = [1,'太白','wusir',['麻花疼',['可口可乐'],'王健林']]

# 找到wusir
print(lst[2])

# 找到太白和wusir
print(lst[1:3])

# 找到太白的白字
print(lst[1][1])

# 将wusir拿到,然后首字母大写 在扔回去

s = lst[2]
s = s.capitalize()
lst[2] = s
print(lst)

# 简写
lst[2] = lst[2].capitalize()
print(lst)

# 把太白换成太黑
lst[1] = lst[1].replace('白','黑')

# 把麻花疼换成麻花不疼
lst[3][0] = lst[3][0].replace('疼','不疼')
print(lst)

# 在可口可乐后边添加一个雪碧
lst[3][1].append('雪碧')
print(lst)
```

**练习**

```PYTHON
写代码，有如下列表，按照要求实现每一个功能。
lis = [2, 3, "k", ["qwe", 20, ["k1", ["tt", 3, "1"]], 89], "ab", "adv"]
将列表lis中的"tt"变成大写（用两种方式）。
将列表中的数字3变成字符串"100"（用两种方式）。
将列表中的字符串"1"变成数字101（用两种方式）。
```

这个知识点用在什么地方:

你需要存储大量的数据，且需要这些数据有序的时候。制定一些特殊的数据群体：按顺序，按规则，自定制设计数据

## 二.元祖

​    1.对于容器型数据类型list，无论谁都可以对其增删改查，那么有一些重要的数据放在list中是不安全的，所以需要一种容器类的数据类型存放重要的数据，创建之初只能查看而不能增删改，这种数据类型就是元祖。

元祖:俗称不可变的列表,又被成为只读列表,元祖也是python的基本数据类型之一,用小括号

括起来,里面可以放任何数据类型的数据,查询可以,循环也可以,切片也可以.但就是不能改.在python中关键字是tuple

```PYTHON
tu = ('我','怎么','这么','可爱')

tu1 = tu[0]  # 记性下标
print(tu1)

for i in tu:
    print(i)  # 进行for循环

tu2 = tu[0:3]
print(tu2)  # 进行切片

结果:
Traceback (most recent call last):
  File "D:/python_object/path2/test.py", line 1286, in <module>
    tu[0] = '你'
NameError: name 'tu' is not defined
```

关于不可变, 注意: 这里元组的不可变的意思是子元素不可变. 而子元素内部的子元素是可以变, 这取决于子元素是否是可变对象.     

元组中如果只有一个元素. 一定要添加一个逗号, 否则就不是元组

```PYTHON
tu = ('meet')
print(type(tu))  #type是查看数据类型

结果:
<class:str>

tu = ('meet',)
print(type(tu))  #type是查看数据类型

结果:
<class:tuple>
```

这个知识点如何使用

 1.可遍历

 2.可切片

 3.有len,count,index方法

### 2.1 元祖嵌套　　

```PYTHON
tu = ('今天姐姐不在家','姐夫和小姨子在客厅聊天',('姐夫问小姨子税后多少钱','小姨子低声说道说和姐夫还提钱'))
tu1 = tu[0]
tu2 = tu[1]
tu3 = tu[2][0]
tu4 = tu[2][1]

print(tu1)
print(tu2)
print(tu3)
print(tu4)
```

```PYTHON
结果:
今天姐姐不在家
姐夫和小姨子在客厅聊天
姐夫问小姨子税后多少钱
小姨子低声说道说和姐夫还提钱
```

在哪里使用

就是将一些非常重要的不可让人改动的数据放在元祖中，只供查看。

## 三.range

翻译过来就是范围,那我们我来先看下.

```PYTHON
range(0,5,1)

参数第一个是范围的起始位置
参数第二个是范围的结束位置
参数第三个是步长
print(range(0,5))
# 结果:
range(0, 5)  #一个范围
```

```PYTHON
# 我们可以通过list方法来转换这个范围来查看一下
l = list(range(0,5))
print(l)

# 结果:
[0, 1, 2, 3, 4]
```

```PYTHON
l = list(range(0,5,2))
print(l)
# 结果:
[0, 2, 4]   # 这个结果就会发现和我之前用步长获取的内容是相识的,是的他就是步长
```

**练习**　　

```PYTHON
利用for循环和range打印出下面列表的索引。

li = ["alex", "WuSir", "ritian", "barry", "wenzhou"]

利用for循环和range找出100以内所有的偶数并将这些偶数插入到一个新列表中

利用for循环和range 找出50以内能被3整除的数，并将这些数插入到一个新列表中

利用for循环和range从100~1，倒序打印

利用for循环和range，将1-30的数字一次添加到一个列表中，并循环这个列表，将能被3整除的数改成*。
```

