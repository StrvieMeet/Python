### 一.re模块

##### 1.什么是正则？

　**正则就是用一些具有特殊含义的符号组合到一起（称为正则表达式）来描述字符或者字符串的方法。或者说：正则就是用来描述一类事物的规则。**（在Python中）它内嵌在Python中，并通过 re 模块实现。正则表达式模式被编译成一系列的字节码，然后由用 C 编写的匹配引擎执行。

| 元字符 | 匹配内容                                                     |
| ------ | ------------------------------------------------------------ |
| \w     | 匹配字母（包含中文）或数字或下划线                           |
| \W     | 匹配非字母（包含中文）或数字或下划线                         |
| \s     | 匹配任意的空白符                                             |
| \S     | 匹配任意非空白符                                             |
| \d     | 匹配数字                                                     |
| \D     | 匹配非数字                                                   |
| \A     | 从字符串开头匹配                                             |
| \z     | 匹配字符串的结束，如果是换行，只匹配到换行前的结果           |
| \n     | 匹配一个换行符                                               |
| \t     | 匹配一个制表符                                               |
| ^      | 匹配字符串的开始                                             |
| $      | 匹配字符串的结尾                                             |
| .      | 匹配任意字符，除了换行符，当re.DOTALL标记被指定时，则可以匹配包括换行符的任意字符。 |
| [...]  | 匹配字符组中的字符                                           |
| [^...] | 匹配除了字符组中的字符的所有字符                             |
| *      | 匹配0个或者多个左边的字符。                                  |
| +      | 匹配一个或者多个左边的字符。                                 |
| ？     | 匹配0个或者1个左边的字符，非贪婪方式。                       |
| {n}    | 精准匹配n个前面的表达式。                                    |
| {n,m}  | 匹配n到m次由前面的正则表达式定义的片段，贪婪方式             |
| ab     | 匹配a或者b                                                   |
| ()     | 匹配括号内的表达式，也表示一个组                             |

**------------------------------------------------匹配模式----------------------------------------------------**

我们现在配合着[正则表达式](https://tool.chinaz.com/regex)来进行测试

1.字符串的常用操作：一对一匹配

```python
s1 = 'meet郭宝元'
print(s1.find('宝元'))
```

2.正则匹配

**\w** 匹配中文,字母,数字,下划线

```python
import re
name = "宝元-meet_123 "
print(re.findall("\w",name))
#结果
['宝', '元', 'm', 'e', 'e', 't', '_', '1', '2', '3']
```

**\W** 不匹配中文,字母,数字,下划线

```python
import re
name = "宝元-meet_123 "
print(re.findall("\W",name))
# 结果
['-',' ']
```

**\s** 匹配任意的空白符

```python
import re
name = "宝元-meet_123 "
print(re.findall("\s",name))
# 结果
[' ']
```

**\S** 匹配不是任意的空白符

```python
import re
name = "宝元-meet_123 "
print(re.findall("\s",name))
# 结果
['宝', '元', '-', 'm', 'e', 'e', 't', '_', '1', '2', '3']
```

**\d** 匹配数字

```python
import re
name = "宝元-meet_123 "
print(re.findall("\d",name))
# 结果
['1', '2', '3']
```

**\D** 匹配非数字

```python
import re
name = "宝元-meet_123 "
print(re.findall("\D",name))
# 结果
['宝', '元', '-', 'm', 'e', 'e', 't', '_', ' ']
```

**\A 与 ^ **从字符串开头匹配

```python
import re
name = "宝元-meet_123 "
print(re.findall("\A宝元",name))
# 结果
['宝元']

import re
name = "宝元-meet_123 "
print(re.findall("\A宝元",name))
# 结果
['宝元']
```

**\Z 与 \z 与 $** 字符串结尾匹配

```python
import re
name = "宝元-meet_123 "
print(re.findall("123 \Z",name))
# 结果
['123 ']

import re
name = "宝元-meet_123 "
print(re.findall("123 \Z",name))
# 结果
['123 ']

import re
name = "宝元-meet_123 "
print(re.findall("123 $",name))
# 结果
['123 ']
```

**\n 与 \t** 匹配换行符合制表符

```python
import re
name = "宝元-meet_123\t \n"
print(re.findall("\n",name))
# 结果
['\n']

import re
name = "宝元-meet_123\t \n"
print(re.findall("\t",name))
# 结果
['\t']
```

**------------------------------------------------匹配方式----------------------------------------------------**

**.**  匹配任意字符(换行符除外)

```python
import re
name = "宝元-meet_123\t \n"
print(re.findall(".",name))
# 结果
['宝', '元', '-', 'm', 'e', 'e', 't', '_', '1', '2', '3', '\t', ' ']
```

**.**  匹配任意字符

```python
import re
name = "宝元-meet_123\t \n"
print(re.findall(".",name,re.DOTALL))
# 结果
['宝', '元', '-', 'm', 'e', 'e', 't', '_', '1', '2', '3', '\t', '\n']
```

**?** 匹配?前元素0个或1个

```python
import re
name = "m-e-me-meet-meet_123\t \n"
print(re.findall("me?",name))
# 结果
['m', 'me', 'me', 'me']
```

***** 匹配 * 前面元素0个或多个 [贪婪匹配]

```python
import re
name = "m-e-me-meet-meet_123\t \n"
print(re.findall("*",name))
# 结果
['m', 'me', 'mee', 'mee']
```

**+** 匹配 +前面元素1个或多个 [贪婪匹配]

```python
import re
name = "m-e-me-meet-meet_123\t \n"
print(re.findall("me+",name))
# 结果
['me', 'mee', 'mee']
```

**{n,m}** 匹配n到m个元素

```python
import re
name = "m-e-me-meet-meet_123\t \n"
print(re.findall("e{1,2}",name))
# 结果
['e', 'e', 'ee', 'ee']
```

**.*** 任意内容0个或多个 

```python
import re
name = "m-e-me-meet-meet_123\t \n"
print(re.findall(".*",name))
# 结果
['m-e-me-meet-meet_123', '']
```

**.*?** 任意内容0个或1个

```python
import re
name = "m-e-me-meet-meet_123"
print(re.findall("m.*?e",name))
# 结果
['m-e', 'me', 'mee', 'mee']

import re
name = "m-e-me-meet-meet_123"
print(re.findall("m.?e",name))
# 结果
['m-e', 'me', 'mee', 'mee']
```

**[]** 获取括号中的内容

```python
import re
name = "m-e-me-meet-meet_123"
print(re.findall("[1-9]",name))
# 结果
['1', '2', '3']
# []中的-是什么至什么不会匹配-

import re
name = "m-e-me-meet-meet_123"
print(re.findall("[a-z]",name))
# 结果
['m', 'e', 'm', 'e', 'm', 'e', 'e', 't', 'm', 'e', 'e', 't']

import re
name = "m-e-me-meet-meet_123"
print(re.findall("[A-z]",name))
# 结果
['m', 'e', 'm', 'e', 'm', 'e', 'e', 't', 'm', 'e', 'e', 't', '_']
# 是按照ascii码表位进行匹配的

import re
name = "m-e-me-meet-meet_123"
print(re.findall("[a-zA-Z]",name))
# 结果
['m', 'e', 'm', 'e', 'm', 'e', 'e', 't', 'm', 'e', 'e', 't']

import re
name = "m-e-me-meet-meet_123"
print(re.findall("[^A-z]",name))
# 结果
['-', '-', '-', '-', '1', '2', '3']
# [^A-z] 有上尖号就是取反,获取不是字母和特定的几个字符

如果想要匹配到-,就需要进行如下操作(将-号放到最前面)
import re
name = "m-e-me-meet-meet_123"
print(re.findall("[-+*/]",name))
# 结果
['-', '-', '-', '-']
```

**练习**

有如下字符串:'alex_sb ale123_sb wu12sir_sb wusir_sb ritian_sb' 的 alex wusir '

找到所有带_sb的内容



**()** 分组 定制一个匹配规则

```python
import re
print(re.findall('(.*?)_sb', 'alex_sb wusir_sb 日天_sb'))
# 结果
['alex', ' wusir', ' 日天']

# 应用举例:
print(re.findall('href="(.*?)"','<a href="http://www.baidu.com">点击</a>')
# 结果
['http://www.baidu.com']
```

**| ** 匹配 左边或者右边

```python
import re
print(re.findall('alex|宝元|wusir', 'alex宝元wusiraleeeex宝宝元odlb'))
# 结果
['alex', '宝元', 'wusir', '宝元']

import re
print(re.findall('compan(day|morrow)','Work harder today than yesterday, and the day after tomorrow will be better'))
# 结果
['day', 'morrow']

import re
print(re.findall('compan(?:day|morrow)','Work harder today than yesterday, and the day after tomorrow will be better'))
# 结果
['today', 'tomorrow']
# 分组() 中加入?: 表示将整体匹配出来而不只是()里面的内容。
```

**------------------------------------------------常用方法----------------------------------------------------**

**findall**   全部找到返回一个列表

```python
import re
print(re.findall("alex","alexdsb,alex_sb,alexnb,al_ex"))
# 结果
['alex', 'alex', 'alex']
```

**search**  从字符串中任意位置进行匹配查找到一个就停止了,返回的是一个对象. 获取匹配的内容必须使用.group()进行获取

```py
import re
print(re.search("sb|nb","alexdsb,alex_sb,alexnb,al_ex").group())
# 结果
sb
```

**match** 从字符串开始位置进行匹配

```python
import re
print(re.match('meet', 'meet alex wusir 日天').group())
# 结果
meet

import re
print(re.match('alex', 'meet alex wusir 日天'))
# 结果
None 
```

**split** 分隔 可按照任意分隔符进行分隔

```python
import re
print(re.split('[ ：:,;；，]','alex wusir,日天，太白;女神;肖锋：吴超'))
# 结果
['alex', 'wusir', '日天', '太白', '女神', '肖锋', '吴超']
```

**sub** 替换

```python
import re
print(re.sub('barry', 'meet', 'barry是最好的讲师，barry就是一个普通老师，请不要将barry当男神对待。'))
# 结果
meet是最好的讲师，meet就是一个普通老师，请不要将meet当男神对待。
```

**compile** 定义匹配规则

```python
import re
obj = re.compile('\d{2}')
print(obj.findall("alex12345"))
# 结果
['12', '34']

import re
['12', '34']
obj = re.compile('\d{2}')
print(obj.search("alex12345").group())
# 结果
12
```

**finditer** 返回一个迭代器

```python
import re
g = re.finditer('al',"alex_alsb,al22,aladf")
print(next(g).group())
print([i.group() for i in g])
# 结果
al
['al','al','al']
```

给分组起名字

```python
import re
ret = re.search("<(?P<tag_name>\w+)>\w+</\w+>","<h1>hello</h1>")
print(ret.group("tag_name"))
print(ret.group())
# 结果
h1
<h1>hello</h1>

import re
ret = re.search(r"<(\w+)>\w+</\1>","<h1>hello</h1>")
print(ret.group(1))
print(ret.group())
```

相关练习:

```python
1 "1-2*(60+(-40.35/5)-(-4*3))"
1.1 匹配所有的整数
print(re.findall('\d+',"1-2*(60+(-40.35/5)-(-4*3))"))
1.2 匹配所有的数字（包含小数）
print(re.findall(r'\d+\.?\d*|\d*\.?\d+', "1-2*(60+(-40.35/5)-(-4*3))"))
1.3 匹配所有的数字（包含小数包含负号）
print(re.findall(r'-?\d+\.?\d*|\d*\.?\d+', "1-2*(60+(-40.35/5)-(-4*3))"))

2,匹配一段你文本中的每行的邮箱
http://blog.csdn.net/make164492212/article/details/51656638 匹配所有邮箱

3，匹配一段你文本中的每行的时间字符串 这样的形式：'1995-04-27'

s1 = '''
时间就是1995-04-27,2005-04-27
1999-04-27 老男孩教育创始人
老男孩老师 alex 1980-04-27:1980-04-27
2018-12-08
'''
print(re.findall('\d{4}-\d{2}-\d{2}', s1))

4 匹配 一个浮点数
print(re.findall('\d+\.\d*','1.17'))

5 匹配qq号：腾讯从10000开始：
print(re.findall('[1-9][0-9]{4,}', '2413545136'))

s1 = '''
<div id="cnblogs_post_body" class="blogpost-body"><h3><span style="font-family: 楷体;">python基础篇</span></h3>
<p><span style="font-family: 楷体;">&nbsp; &nbsp;<strong><a href="http://www.cnblogs.com/guobaoyuan/p/6847032.html" target="_blank">python 基础知识</a></strong></span></p>
<p><span style="font-family: 楷体;"><strong>&nbsp; &nbsp;<a href="http://www.cnblogs.com/guobaoyuan/p/6627631.html" target="_blank">python 初始python</a></strong></span></p>
<p><span style="font-family: 楷体;"><strong>&nbsp; &nbsp;<strong><a href="http://www.cnblogs.com/guobaoyuan/articles/7087609.html" target="_blank">python 字符编码</a></strong></strong></span></p>
<p><span style="font-family: 楷体;"><strong><strong>&nbsp; &nbsp;<a href="http://www.cnblogs.com/guobaoyuan/articles/6752157.html" target="_blank">python 类型及变量</a></strong></strong></span></p>
<p><span style="font-family: 楷体;"><strong>&nbsp; &nbsp;<a href="http://www.cnblogs.com/guobaoyuan/p/6847663.html" target="_blank">python 字符串详解</a></strong></span></p>
<p><span style="font-family: 楷体;">&nbsp; &nbsp;<strong><a href="http://www.cnblogs.com/guobaoyuan/p/6850347.html" target="_blank">python 列表详解</a></strong></span></p>
<p><span style="font-family: 楷体;"><strong>&nbsp; &nbsp;<a href="http://www.cnblogs.com/guobaoyuan/p/6850496.html" target="_blank">python 数字元祖</a></strong></span></p>
<p><span style="font-family: 楷体;">&nbsp; &nbsp;<strong><a href="http://www.cnblogs.com/guobaoyuan/p/6851820.html" target="_blank">python 字典详解</a></strong></span></p>
<p><span style="font-family: 楷体;"><strong>&nbsp; &nbsp;<strong><a href="http://www.cnblogs.com/guobaoyuan/p/6852131.html" target="_blank">python 集合详解</a></strong></strong></span></p>
<p><span style="font-family: 楷体;"><strong>&nbsp; &nbsp;<a href="http://www.cnblogs.com/guobaoyuan/articles/7087614.html" target="_blank">python 数据类型</a>&nbsp;</strong></span></p>
<p><span style="font-family: 楷体;"><strong>&nbsp; &nbsp;<a href="http://www.cnblogs.com/guobaoyuan/p/6752169.html" target="_blank">python文件操作</a></strong></span></p>
<p><span style="font-family: 楷体;"><strong>&nbsp; &nbsp;<a href="http://www.cnblogs.com/guobaoyuan/p/8149209.html" target="_blank">python 闭包</a></strong></span></p>
<p><span style="font-family: 楷体;"><strong>&nbsp; &nbsp;<a href="http://www.cnblogs.com/guobaoyuan/articles/6705714.html" target="_blank">python 函数详解</a></strong></span></p>
<p><span style="font-family: 楷体;"><strong>&nbsp; &nbsp;<a href="http://www.cnblogs.com/guobaoyuan/articles/7087616.html" target="_blank">python 函数、装饰器、内置函数</a></strong></span></p>
<p><span style="font-family: 楷体;"><strong>&nbsp; &nbsp;<a href="http://www.cnblogs.com/guobaoyuan/articles/7087629.html" target="_blank">python 迭代器 生成器</a>&nbsp;&nbsp;</strong></span></p>
<p><span style="font-family: 楷体;"><strong>&nbsp; &nbsp;<a href="http://www.cnblogs.com/guobaoyuan/articles/6757215.html" target="_blank">python匿名函数、内置函数</a></strong></span></p>
</div>
'''
1,找到所有的span标签的内容
ret = re.findall('<span(.*?)>', s1)
print(ret)

2,找到所有a标签对应的url
print(re.findall('<a href="(.*?)".*?</a>',s1))
```

