[TOC]



# Python基础数据类型补充

## 一.二次编码

编码回顾:

ASCII : 最早的编码. ⾥⾯有英⽂⼤写字⺟, ⼩写字⺟, 数字, ⼀些特殊字符.
没有中⽂, 8个01代码, 8个bit, 1个byte

GBK: 中⽂国标码, ⾥⾯包含了ASCII编码和中⽂常⽤编码. 16个bit, 2个byte

UNICODE: 万国码, ⾥⾯包含了全世界所有国家⽂字的编码. 32个bit, 4个byte, 包含了 ASCII

UTF-8: 可变⻓度的万国码. 是unicode的⼀种实现. 最⼩字符占8位
　　1.英⽂: 8bit 1byte
　　2.欧洲⽂字:16bit 2byte
　　3.中⽂:24bit 3byte

综上, 除了ASCII码以外, 其他信息不能直接转换.
在python3的内存中. 在程序运⾏阶段. 使⽤的是unicode编码.
因为unicode是万国码. 什么内容都可以进⾏显⽰. 那么在数据传输和存储的时候由于unicode比较浪费空间和资源.
需要把 unicode转存成UTF-8或者GBK进⾏存储. 怎么转换呢.
在python中可以把⽂字信息进⾏编码. 编码之后的内容就可以进⾏传输了.
编码之后的数据是bytes类型的数据.其实啊. 还是原来的 数据只是经过编码之后表现形式发⽣了改变⽽已.

bytes的表现形式:

  1. 英⽂ b'alex' 英⽂的表现形式和字符串没什么两样
  2. 中⽂ b'\xe4\xb8\xad' 这是⼀个汉字的UTF-8的bytes表现形式

### 1.1 编码

```
s = "alex"
print(s.encode("utf-8")) # 将字符串编码成UTF-8
print(s.encode("GBK")) # 将字符串编码成GBK
结果:
b'alex'
b'alex'
s = "中"
print(s.encode("UTF-8")) # 中⽂编码成UTF-8
print(s.encode("GBK")) # 中⽂编码成GBK
结果:
b'\xe4\xb8\xad'
b'\xd6\xd0'
```

记住: 英⽂编码之后的结果和源字符串⼀致. 中⽂编码之后的结果根据编码的不同. 编码结果也不同.
　　  我们能看到. ⼀个中⽂的UTF-8编码是3个字节. ⼀个GBK的中⽂编码是2个字节.
　　  编码之后的类型就是bytes类型. 在⽹络传输和存储的时候我们python是保存和存储的bytes 类型.
　　  那么在对⽅接收的时候. 也是接收的bytes类型的数据. 我们可以使⽤decode()来进⾏解 码操作.
　　 把bytes类型的数据还原回我们熟悉的字符串:

```
s = "我叫李嘉诚"
print(s.encode("utf-8")) #
b'\xe6\x88\x91\xe5\x8f\xab\xe6\x9d\x8e\xe5\x98\x89\xe8\xaf\x9a'
print(b'\xe6\x88\x91\xe5\x8f\xab\xe6\x9d\x8e\xe5\x98\x89\xe8\xaf\x9a'.decod
e("utf-8")) # 解码
```

### 1.2 编码和解码

编码和解码的时候都需要制定编码格式. 

```
s = "我是⽂字" bs = s.encode("GBK") 
# 我们这样可以获取到GBK的⽂字 
# 把GBK转换成UTF-8 
# ⾸先要把GBK转换成unicode. 也就是需要解码 
s = bs.decode("GBK") # 解码 
# 然后需要进⾏重新编码成UTF-8 
bss = s.encode("UTF-8") # 重新编码 
print(bss)
```

## 二.数据补充

我们补充给几个数据类型的操作

### 2.1 循环添加

```
lst = [1,2,3,4,5,6]
for i in lst:
    lst.append(7) # 这样写法就会一直持续添加7
    print(lst)
print(lst)   
```

### 2.2 列表循环删除

列表: 循环删除列表中的每⼀个元素

```
li = [11, 22, 33, 44]
for e in li:
 li.remove(e)
print(li)
结果:
[22, 44]
```

分析原因: for的运⾏过程. 会有⼀个指针来记录当前循环的元素是哪⼀个, ⼀开始这个指针指向第0 个.

然后获取到第0个元素. 紧接着删除第0个. 这个时候. 原来是第⼀个的元素会⾃动的变成 第0个.

然后指针向后移动⼀次, 指向1元素. 这时原来的1已经变成了0, 也就不会被删除了.

⽤pop删除试试看:

```
li = [11, 22, 33, 44]
for i in range(0, len(li)):
 del li[i]
print(li)
结果: 报错
# i= 0, 1, 2 删除的时候li[0] 被删除之后. 后⾯⼀个就变成了第0个.
# 以此类推. 当i = 2的时候. list中只有⼀个元素. 但是这个时候删除的是第2个 肯定报错啊
```

经过分析发现. 循环删除都不⾏. 不论是⽤del还是⽤remove. 都不能实现. 那么pop呢?

```
for el in li:
 li.pop() # pop也不⾏
print(li)
结果:
[11, 22]
```

### 2.3 列表循环删除成功

只有这样才是可以的:

```
for i in range(0, len(li)): # 循环len(li)次, 然后从后往前删除
 li.pop()
print(li)
或者. ⽤另⼀个列表来记录你要删除的内容. 然后循环删除

li = [11, 22, 33, 44]
del_li = []
for e in li:
 del_li.append(e)
for e in del_li:
 li.remove(e)

print(li)
```

注意: 由于删除元素会导致元素的索引改变, 所以容易出现问题. 尽量不要再循环中直接去删 除元素. 可以把要删除的元素添加到另⼀个集合中然后再批量删除.

### 2.4 字典的坑

dict中的fromkey(),再次重提 可以帮我们通过list来创建⼀个dict

```
dic = dict.fromkeys(["jay", "JJ"], ["周杰伦", "麻花藤"])
print(dic)
结果:
{'jay': ['周杰伦', '麻花藤'], 'JJ': ['周杰伦', '麻花藤']}
```

代码中只是更改了jay那个列表. 但是由于jay和JJ⽤的是同⼀个列表. 所以. 前⾯那个改了.  后面那个也会跟着改　

### 2.5 字典在循环时不能修改

dict中的元素在迭代过程中是不允许进⾏删除的

```
dic = {'k1': 'alex', 'k2': 'wusir', 's1': '⾦⽼板'}
# 删除key中带有'k'的元素
for k in dic:
  if 'k' in k:
 del dic[k] # dictionary changed size during iteration, 在循环迭
代的时候不允许进⾏删除操作
print(dic)
```

那怎么办呢? 把要删除的元素暂时先保存在⼀个list中, 然后循环list, 再删除

```
dic = {'k1': 'alex', 'k2': 'wusir', 's1': '⾦⽼板'}
dic_del_list = []
# 删除key中带有'k'的元素
for k in dic:
 if 'k' in k:
 dic_del_list.append(k)
for el in dic_del_list:
 del dic[el]
print(dic)
```

### 2.6 类型转换

```
　　元组 => 列表 list(tuple)

　　列表 => 元组 tuple(list)

　　list=>str str.join(list)

　　str=>list str.split()

　　转换成False的数据:

　　 0,'',None,[],(),{},set() ==> False
```

## 三.深浅拷贝

### 3.1 赋值

```
lst1 = ["⾦⽑狮王", "紫衫⻰王", "⽩眉鹰王", "⻘翼蝠王"]
lst2 = lst1
print(lst1)
print(lst2)
lst1.append("杨逍")
print(lst1)
print(lst2)
结果:
['⾦⽑狮王', '紫衫⻰王', '⽩眉鹰王', '⻘翼蝠王', '杨逍']
['⾦⽑狮王', '紫衫⻰王', '⽩眉鹰王', '⻘翼蝠王', '杨逍']
```

上边的lst1和lst2使用是一个列表,对lst1进行操作后lst2也会发生变化

这个就像是小明和小刚两个人在和一杯水,小明喝了一口,小刚在喝的时候就会少一点.小刚把杯子里的水加满了以后

小明在喝水的时候水就变多了.

```
dic1 = {"id": 123, "name": "谢逊"}
dic2 = dic1
print(dic1)
print(dic2)
dic1['name'] = "范瑶"
print(dic1)
print(dic2)
结果:
{'id': 123, 'name': '谢逊'}
{'id': 123, 'name': '谢逊'}
{'id': 123, 'name': '范瑶'}
{'id': 123, 'name': '范瑶'}
```

这个和上边的列表是同样的道理

对于list, set, dict来说, 直接赋值. 其实是把内存地址交给变量. 并不是复制⼀份内容. 所以. lst1的内存指向和lst2是⼀样的. lst1改变了, lst2也发⽣了改变　　

### 3.2 浅拷⻉

```
lst1 = ["何炅", "杜海涛","周渝⺠"]
lst2 = lst1.copy()
lst1.append("李嘉诚")
print(lst1)
print(lst2)
print(id(lst1), id(lst2))

结果:
两个lst完全不⼀样. 内存地址和内容也不⼀样. 发现实现了内存的拷⻉
lst1 = ["何炅", "杜海涛","周渝⺠", ["麻花藤", "⻢芸", "周笔畅"]]
lst2 = lst1.copy()
lst1[3].append("⽆敌是多磨寂寞")
print(lst1)
print(lst2)
print(id(lst1[3]), id(lst2[3]))
结果:
['何炅', '杜海涛', '周渝⺠', ['麻花藤', '⻢芸', '周笔畅', '⽆敌是多磨寂寞']]
['何炅', '杜海涛', '周渝⺠', ['麻花藤', '⻢芸', '周笔畅', '⽆敌是多磨寂寞']]
4417248328 4417248328
```

浅拷⻉. 只会拷⻉第⼀层. 第⼆层的内容不会拷⻉. 所以被称为浅拷⻉

### 3.3 深拷⻉

```
import copy
lst1 = ["何炅", "杜海涛","周渝⺠", ["麻花藤", "⻢芸", "周笔畅"]]
lst2 = copy.deepcopy(lst1)
lst1[3].append("⽆敌是多磨寂寞")
print(lst1)
print(lst2)
print(id(lst1[3]), id(lst2[3]))
结果:
['何炅', '杜海涛', '周渝⺠', ['麻花藤', '⻢芸', '周笔畅', '⽆敌是多磨寂寞']]
['何炅', '杜海涛', '周渝⺠', ['麻花藤', '⻢芸', '周笔畅']]
4447221448 4447233800
```

都不⼀样了.

深度拷贝. 把元素内部的元素完全进行拷贝复制. 不会产⽣⼀个改变另⼀个跟着 改变的问题 

## 四. id is  ==

在Python中，id是什么？id是内存地址，比如你利用id()内置函数去查询一个数据的内存地址：
name = 'meet'
s_id = id(name)   # 通过内置方法获取name变量对应的值在内存中的编号
print(s_id)       # 2055782908568 这就是name在内存中的编号
那么 is 是什么？ == 又是什么？
== 是比较的两边的数值是否相等，而 is 是比较的两边的内存地址是否相等。 如果内存地址相等，那么这两边其实是指向同一个内存地址。


