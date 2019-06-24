# Python基础数据类型三

## 一.字典

列表可以存储大量的数据类型，但是只能按照顺序存储，数据与数据之间关联性不强。

所以咱们需要引入一种容器型的数据类型，解决上面的问题，这就需要dict字典。

字典(dict)是python中唯⼀的⼀个映射类型.他是以{ }括起来的键值对组成.

在dict中key是 唯⼀的.在保存的时候, 根据key来计算出⼀个内存地址. 然后将key-value保存在这个地址中.

这种算法被称为hash算法, 所以, 切记, 在dict中存储的key-value中的key必须是可hash的

可以改变的都是不可哈希的, 那么可哈希就意味着不可变. 这个是为了能准确的计算内存地址⽽规定的.

已知的可哈希(不可变)的数据类型: int, str, tuple, bool 不可哈希(可变)的数据类型: list, dict, set

```
语法:{'key1':1,'key2':2}
```

注意: key必须是不可变(可哈希)的. value没有要求.可以保存任意类型的数据

```python
# 合法
dic = {123: 456, True: 999, "id": 1, "name": 'sylar', "age": 18, "stu": ['帅
哥', '美⼥'], (1, 2, 3): '麻花藤'}
print(dic[123])
print(dic[True])
print(dic['id'])
print(dic['stu'])
print(dic[(1, 2, 3)])

# 不合法
# dic = {[1, 2, 3]: '周杰伦'} # list是可变的. 不能作为key
# dic = {{1: 2}: "哈哈哈"} # dict是可变的. 不能作为key
dic = {{1, 2, 3}: '呵呵呵'} # set是可变的, 不能作为key
```

注意:dict保存的数据不是按照我们添加进去的顺序保存的. 是按照hash表的顺序保存的. ⽽hash表 不是连续的. 所以不能进⾏切片⼯作. 它只能通过key来获取dict中的数据

### 1.1 字典增删改查

#### 1.1.1 增

```python
dic = {}

dic['name'] = '汪峰'
dic['age'] = 18
print(dic)

结果:
{'name': '汪峰', 'age': 18}
# 如果dict中没有出现这个key,就会将key-value组合添加到这个字典中

# 如果dict中没有出现过这个key-value. 可以通过setdefault设置默认值
s1 = dic.setdefault('王菲')
print(s1)
print(dic)
结果:
None    
# 返回的是添加进去的值
{'王菲': None}  
# 我们使用setdefault这个方法 里边放的这个内容是我们字典的健,这样我们添加出来的结果
就是值是一个None

dic.setdefault('王菲',歌手)    
# 这样就是不会进行添加操作了,因为王菲在dic这个字典中存在
# 总结: 当setdefault中第一个参数存在这个字典中就就不进行添加操作,否则就添加

dic1 = {}
s2 = dic1.setdefault('王菲','歌手')
print(s2)
print(dic1)
结果: 
歌手
{'王菲': '歌手'}
```

#### 1.1.2删

```python
dic = {'剑圣':'易','哈啥给':'剑豪','大宝剑':'盖伦'}
s = dic.pop('哈啥给')   # pop删除有返回值,返回的是被删的值
print(s)

print(dic)    # 打印删除后的字典
dic.popitem()  # 随机删除  python3.6是删除最后一个
print(dic)

dic.clear()  # 清空
```

#### 1.1.3改　　

```python
dic = {'剑圣':'易','哈啥给':'剑豪','大宝剑':'盖伦'}
dic['哈啥给'] = '剑姬'   # 当哈哈给是字典中的健这样写就是修改对应的值,如果不存在就是添加

print(dic)
dic.update({'key':'v','哈啥给':'剑姬'})

# 当update中的字典里没有dic中键值对就添加到dic字典中,如果有就修改里边的对应的值
print(dic)
```

#### 1.1.4 查

```
dic = {'剑圣':'易','哈啥给':'剑豪','大宝剑':'盖伦'}
s = dic['大宝剑']        #通过健来查看,如果这个健不在这个字典中.就会报错
print(s)

s1 = dic.get('剑圣')     #通过健来查看,如果这个健不在这个字典中.就会返回None
print(s1)

s2 = dic.get('剑姬','没有还查你是不是傻')  # 我们可以在get查找的时候自己定义返回的结果
print(s2)
```

**练习**

```
dic = {'k1': "v1", "k2": "v2", "k3": [11,22,33]}
请在字典中添加一个键值对，"k4": "v4"，输出添加后的字典
请在修改字典中 "k1" 对应的值为 "alex"，输出修改后的字典
请在k3对应的值中追加一个元素 44，输出修改后的字典
请在k3对应的值的第 1 个位置插入个元素 18，输出修改后的字典
```

### 1.2字典其他操作

### 1.2.1 获取字典所有的键

```
key_list = dic.keys()    
print(key_list)

结果:
dict_keys(['剑圣', '哈啥给', '大宝剑'])
# 一个高仿列表,存放的都是字典中的key

```

### 1.2.2 获取字典所有的值

```
value_list = dic.values()
print(value_list)

结果:
dict_values(['易', '剑豪', '盖伦'])
#一个高仿列表,存放都是字典中的value　
```

### 1.2.3 获取字典的键值对

```
key_value_list = dic.items()
print(key_value_list)
结果:
dict_items([('剑圣', '易'), ('哈啥给', '剑豪'), ('大宝剑', '盖伦')])

# 一个高仿列表,存放是多个元祖,元祖中第一个是字典中的键,第二个是字典中的值　
```

**练习**

循环打印字典的值

循环打印字典的键

循环打印元祖形式的键值对

```
dic = {'剑圣':'易','哈啥给':'剑豪','大宝剑':'盖伦'}

for i in dic:
    print(i)

for i in dic.keys():
    print(i)
```

 循环打印字典中的键

```
dic = {'剑圣':'易','哈啥给':'剑豪','大宝剑':'盖伦'}
for i in dic:
    print(dic[i])
    
for i in dic.values():   
    print(i)
```

循环打印字典中的值和值

```
dic = {'剑圣':'易','哈啥给':'剑豪','大宝剑':'盖伦'}
for i in dic.items():
    print(i)
```

循环打印元祖形式的键值对

### 1.3 解构

```
a,b = 1,2
print(a,b)
结果:
1 2

a,b = ('你好','世界')
print(a,b)
结果:
你好 世界

a,b = ['你好','大飞哥']
print(a,b)
结果:
你好 世界

a,b = {'汪峰':'北京北京','王菲':'天后'}
print(a,b)
结果:
汪峰 王菲
```

解构可以将内容分别赋值到变量当中,我们使用解构就能够快速的将值使用

#### 1.3.1 循环字典获取键和值

```
for k,v in dic.items():
    print('这是键',k)
    print('这是值',v)

结果:
这是键 剑圣
这是值 易
这是键 哈啥给
这是值 剑豪
这是键 大宝剑
这是值 盖伦
```

### 1.4 字典的嵌套

```
dic = {
    'name':'汪峰',
    'age':48,
    'wife':[{'name':'国际章','age':38}],
    'children':['第一个熊孩子','第二个熊孩子']
}
```

获取汪峰的妻子名字

```
d1 = dic['wife'][0]['name']
print(d1)
```

获取汪峰的孩子们

```
d2 = dic['children']
print(d2)
```

获取汪峰的第一个孩子

```
d3 = dic['children'][0]
print(d3)
```

**练习**

```
dic1 = {
 'name':['alex',2,3,5],
 'job':'teacher',
 'oldboy':{'alex':['python1','python2',100]}
 }
1，将name对应的列表追加⼀个元素’wusir’。
2，将name对应的列表中的alex⾸字⺟⼤写。
3，oldboy对应的字典加⼀个键值对’⽼男孩’,’linux’。
4，将oldboy对应的字典中的alex对应的列表中的python2删除
```

