[TOC]



# Python 模块二

## 一. re

re模块大家都叫它正则,那什么又是正则呢?

正则就是用一些具有特殊含义的符号组合到一起（称为正则表达式）来描述字符或者字符串的方法。或者说：正则就是用来描述一类事物的规则。（在Python中）它内嵌在Python中，并通过 re 模块实现。正则表达式模式被编译成一系列的字节码，然后由用 C 编写的匹配引擎执行。

![1548485254556](C:\Users\by\Desktop\大纲\assets\1548485254556.png)

### 1.1 匹配模式举例

```
# ----------------匹配模式--------------------
# 1,之前学过的字符串的常用操作：一对一匹配
# s1 = 'fdskahf太白金星'
# print(s1.find('太白'))  # 7
```

正则匹配：

#### 1.1.1 W

```
# 单个字符匹配
import re
# \w 与 \W
# print(re.findall('\w', '太白jx 12*() _'))  # ['太', '白', 'j', 'x', '1', '2', '_']
# print(re.findall('\W', '太白jx 12*() _'))  # [' ', '*', '(', ')', ' ']
```

#### 1.1.2 S

```
import re
# \s 与\S
# print(re.findall('\s','太白barry*(_ \t \n'))  # [' ', '\t', ' ', '\n']
# print(re.findall('\S','太白barry*(_ \t \n'))  # ['太', '白', 'b', 'a', 'r', 'r', 'y', '*', '(', '_']
```

#### 1.1.3 D

```
import re
# \d 与 \D
# print(re.findall('\d','1234567890 alex *（_'))  # ['1', '2', '3', '4', '5', '6', '7', '8', '9', '0']
# print(re.findall('\D','1234567890 alex *（_'))  # [' ', 'a', 'l', 'e', 'x', ' ', '*', '（', '_']
```

#### 1.1.4 ^

```
import re
# \A 与 ^
# print(re.findall('\Ahel','hello 太白金星 -_- 666'))  # ['hel']
# print(re.findall('^hel','hello 太白金星 -_- 666'))  # ['hel']
```

#### 1.1.5 $

```
# \Z、\z 与 $ 
# print(re.findall('666\Z','hello 太白金星 *-_-* \n666'))  # ['666']
# print(re.findall('666\z','hello 太白金星 *-_-* \n666'))  # []
# print(re.findall('666$','hello 太白金星 *-_-* \n666'))  # ['666']
```

#### 1.1.6 \n

```
# \n 与 \t
# print(re.findall('\n','hello \n 太白金星 \t*-_-*\t \n666'))  # ['\n', '\n']
# print(re.findall('\t','hello \n 太白金星 \t*-_-*\t \n666'))  # ['\t', '\t']
```

### 1.2.重复匹配

```
 . ? * + {m,n} .* .*?

# . 匹配任意字符，除了换行符（re.DOTALL 这个参数可以匹配\n）。
# print(re.findall('a.b', 'ab aab a*b a2b a牛b a\nb'))  # ['aab', 'a*b', 'a2b', 'a牛b']
# print(re.findall('a.b', 'ab aab a*b a2b a牛b a\nb',re.DOTALL))  # ['aab', 'a*b', 'a2b', 'a牛b']


# ？匹配0个或者1个由左边字符定义的片段。
# print(re.findall('a?b', 'ab aab abb aaaab a牛b aba**b'))  # ['ab', 'ab', 'ab', 'b', 'ab', 'b', 'ab', 'b']


# * 匹配0个或者多个左边字符表达式。 满足贪婪匹配 @@
# print(re.findall('a*b', 'ab aab aaab abbb'))  # ['ab', 'aab', 'aaab', 'ab', 'b', 'b']
# print(re.findall('ab*', 'ab aab aaab abbbbb'))  # ['ab', 'a', 'ab', 'a', 'a', 'ab', 'abbbbb']


# + 匹配1个或者多个左边字符表达式。 满足贪婪匹配  @@
# print(re.findall('a+b', 'ab aab aaab abbb'))  # ['ab', 'aab', 'aaab', 'ab']


# {m,n}  匹配m个至n个左边字符表达式。 满足贪婪匹配  @@
# print(re.findall('a{2,4}b', 'ab aab aaab aaaaabb'))  # ['aab', 'aaab']


# .* 贪婪匹配 从头到尾.
# print(re.findall('a.*b', 'ab aab a*()b'))  # ['ab aab a*()b']


# .*? 此时的?不是对左边的字符进行0次或者1次的匹配,
# 而只是针对.*这种贪婪匹配的模式进行一种限定:告知他要遵从非贪婪匹配 推荐使用!
# print(re.findall('a.*?b', 'ab a1b a*()b, aaaaaab'))  # ['ab', 'a1b', 'a*()b']


# []: 括号中可以放任意一个字符,一个中括号代表一个字符
# - 在[]中表示范围,如果想要匹配上- 那么这个-符号不能放在中间.
# ^ 在[]中表示取反的意思.
# print(re.findall('a.b', 'a1b a3b aeb a*b arb a_b'))  # ['a1b', 'a3b', 'a4b', 'a*b', 'arb', 'a_b']
# print(re.findall('a[abc]b', 'aab abb acb adb afb a_b'))  # ['aab', 'abb', 'acb']
# print(re.findall('a[0-9]b', 'a1b a3b aeb a*b arb a_b'))  # ['a1b', 'a3b']
# print(re.findall('a[a-z]b', 'a1b a3b aeb a*b arb a_b'))  # ['aeb', 'arb']
# print(re.findall('a[a-zA-Z]b', 'aAb aWb aeb a*b arb a_b'))  # ['aAb', 'aWb', 'aeb', 'arb']
# print(re.findall('a[0-9][0-9]b', 'a11b a12b a34b a*b arb a_b'))  # ['a11b', 'a12b', 'a34b']
# print(re.findall('a[*-+]b','a-b a*b a+b a/b a6b'))  # ['a*b', 'a+b']
# - 在[]中表示范围,如果想要匹配上- 那么这个-符号不能放在中间.
# print(re.findall('a[-*+]b','a-b a*b a+b a/b a6b'))  # ['a-b', 'a*b', 'a+b']
# print(re.findall('a[^a-z]b', 'acb adb a3b a*b'))  # ['a3b', 'a*b']
```

### 1.3 分组

```
# () 制定一个规则,将满足规则的结果匹配出来

# print(re.findall('(.*?)_sb', 'alex_sb wusir_sb 日天_sb'))  # ['alex', ' wusir', ' 日天']

# 应用举例:

# print(re.findall('href="(.*?)"','<a href="http://www.baidu.com">点击</a>'))#['http://www.baidu.com']
```

### 1.4 或

```
# | 匹配 左边或者右边
# print(re.findall('alex|太白|wusir', 'alex太白wusiraleeeex太太白odlb'))  # ['alex', '太白', 'wusir', '太白']
# print(re.findall('compan(y|ies)','Too many companies have gone bankrupt, and the next one is my company'))  # ['ies', 'y']
# print(re.findall('compan(?:y|ies)','Too many companies have gone bankrupt, and the next one is my company'))  # ['companies', 'company']
# 分组() 中加入?: 表示将整体匹配出来而不只是()里面的内容。
```

### 1.5 常用方法举例

```
#1 findall 全部找到返回一个列表。
# print(re.findall('a', 'alexwusirbarryeval'))  # ['a', 'a', 'a']


# 2 search 只到找到第一个匹配然后返回一个包含匹配信息的对象,该对象可以通过调用group()方法得到匹配的字符串,如果字符串没有匹配，则返回None。
# print(re.search('sb|alex', 'alex sb sb barry 日天'))  # <_sre.SRE_Match object; span=(0, 4), match='alex'>
# print(re.search('alex', 'alex sb sb barry 日天').group())  # alex


# 3 match：None,同search,不过在字符串开始处进行匹配,完全可以用search+^代替match
# print(re.match('barry', 'barry alex wusir 日天'))  # <_sre.SRE_Match object; span=(0, 5), match='barry'>
# print(re.match('barry', 'barry alex wusir 日天').group()) # barry


# 4 split 分割 可按照任意分割符进行分割
# print(re.split('[ ：:,;；，]','alex wusir,日天，太白;女神;肖锋：吴超'))  # ['alex', 'wusir', '日天', '太白', '女神', '肖锋', '吴超']


# 5 sub 替换

# print(re.sub('barry', '太白', 'barry是最好的讲师，barry就是一个普通老师，请不要将barry当男神对待。'))
# 太白是最好的讲师，太白就是一个普通老师，请不要将太白当男神对待。
# print(re.sub('barry', '太白', 'barry是最好的讲师，barry就是一个普通老师，请不要将barry当男神对待。',2))
# 太白是最好的讲师，太白就是一个普通老师，请不要将barry当男神对待。
# print(re.sub('([a-zA-Z]+)([^a-zA-Z]+)([a-zA-Z]+)([^a-zA-Z]+)([a-zA-Z]+)', r'\5\2\3\4\1', r'alex is sb'))
# sb is alex

# 6 重复使用
# obj=re.compile('\d{2}')

# print(obj.search('abc123eeee').group()) #12
# print(obj.findall('abc123eeee')) #['12'],重用了obj


# import re
# ret = re.finditer('\d', 'ds3sy4784a')   #finditer返回一个存放匹配结果的迭代器
# print(ret)  # <callable_iterator object at 0x10195f940>
# print(next(ret).group())  #查看第一个结果
# print(next(ret).group())  #查看第二个结果
# print([i.group() for i in ret])  #查看剩余的左右结果
```

### 1.6 命名分组举例（了解）

```
# 命名分组匹配：
ret = re.search("<(?P<tag_name>\w+)>\w+</(?P=tag_name)>","<h1>hello</h1>")
# #还可以在分组中利用?<name>的形式给分组起名字
# #获取的匹配结果可以直接用group('名字')拿到对应的值
# print(ret.group('tag_name'))  #结果 ：h1
# print(ret.group())  #结果 ：<h1>hello</h1>
#
# ret = re.search(r"<(\w+)>\w+</\1>","<h1>hello</h1>")
# #如果不给组起名字，也可以用\序号来找到对应的组，表示要找的内容和前面的组内容一致
# #获取的匹配结果可以直接用group(序号)拿到对应的值
# print(ret.group(1)) #结果: hello
# print(ret.group())  #结果 ：<h1>hello</h1>
```