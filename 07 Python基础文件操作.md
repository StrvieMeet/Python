[TOC]

# Python基础-文件操作

## 一.只读

有如下文件，但是没有相应的软件打开，想不想看？    

​      美女模特空姐护士联系方式.txt
让你开发一个软件，可以打开此文件，你需要什么参数？
​      文件路径: D:\美女模特空姐护士联系方式.txt
​      编码：utf-8，gbk，gb2312....
​      模式：只读，只写，追加，写读，读写....

使用Python来读写文件是非常简单的操作,我们使用open()来打开一个文件,获取到文件句柄,然后通过文件句柄就可以进行各种各样的操作了

根据打开方式的不同能够执行的操作会有相应的差异.

打开文件的方式:

　　r,w,a

　　r+,w+,a+

　　rb,wb,ab

　　r+b,w+b,a+b

### 1.1 r模式

我们先来看看这个只读操作,听名字应该也知道只能进行读不能进行别的操作

```
f = open('美女模特空姐护士联系方式.txt',mode='r',encoding='utf-8')
content = f.read()
print(content)
f.close()

结果:
标题很好
```

open中第一个参数放入的是要打开的文件名字,第二个参数是要对这个文件进行的操作,第三参数是用什么编码方式打开文件中的内容

![](\assets\1-1548381849319.gif)

f 可写成任意变量等，它被称作：文件句柄，文件操作符，或者文件操作对象等。
open 是Python调用的操作系统（windows,linux,等）的功能，而windows的默认编码方式为gbk，linux默认编码方式为utf-8，所以你的文件用什么编码保存的，就用什么方法打开，一般都是用utf-8。
mode为打开方式：常见的有r,w,a,r+,w+,a+.rb,wb,ab,等，默认不写是r。
流程就是打开文件，产生一个文件句柄，对文件句柄进行相应操作，关闭文件。

### 1.2 rb 模式

我们试了r只读,再看看rb只读字节的模式

```
f = open('护士少妇萝莉',mode='rb')
content = f.read()
print(content)
f.close()

结果:
b'\xe6\xa0\x87\xe9\xa2\x98\xe5\xbe\x88\xe5\xa5\xbd'
```

rb 读出来的数据是bytes类型,在rb模式下,不能encoding字符集

![](\assets\1-1548382195054.gif)

rb的作用:在读取非文本文件的时候,比如要读取mp3,图像,视频等信息的时候就需要用到rb,因为这种数据是没办法直接显示出来的

这个字节的模式是用于传输和存储

好了,我们动手试一试.读文件吧,你们如果是在windows系统中用记事本创建的文件,在打开的时候会有问题,

是因为windows系统中记事本中写的内容编码是GBK我们要不将文件中编码改成utf-8 要不在open中第三个参数 中

修改成utf-8

## 二.路径

### 2.1 相对路劲

以上代码中文件名使用的是相对路劲,什么是相对路劲呢???

![1548382792126](\assets\1548382792126.png)

就是相对于某个文件来说,进行路劲查找,以上代码是相对于运行的文件.

如果相对还是太理解,来看下这列子:

你朋友要来找你,知道你在那个楼那一层但是不知道那个一个屋,现在你朋友来到这个楼里相对他知道的这一层然后开始找你在那个房间

这种操作就是相对路劲,例子中是通过这个楼中的这一层开始寻找,也就是相对于这个楼的某一层

### 2.2 绝对路径

![](assets\1-1548383140604.gif)

绝对路径是从咱们电脑的磁盘开始查找,例如想要知道你的家乡是哪的,你身份证上写的住址就是绝对路径

```
绝对路径：从根目录下开始一直到文件名。
相对路径：同一个文件夹下面的文件，直接写文件名就可以。
```

我们在使用绝对路径的时候因为有\这样程序是不能识别的,解决方法:

```
open('C:\Users\Meet')  #这样程序是不识别的

解决方法一:
open('C:\\Users\\Meet') #这样就成功的将\进行转义   两个\\代表一个\

解决方法二:
open(r'C:\Users\Meet') #这样相比上边的还要省事,在字符串的前面加个小r也是转义的意思
```

我更推荐大家使用相对路劲,因为我把这个程序的整个文件发给你的时候,就可以运行,如果使用绝对路径还需要额外的拷贝外部文件给你

## 三.读操作

### 3.1 read()

read()是将文件中所有的内容都读取

```
f = open('path1/小娃娃.txt',mode='r',encoding='utf-8')
msg = f.read()
f.close()
print(msg)

结果:
高圆圆
刘亦菲
张柏芝
杨紫
王菲
```

read()可以指定我们想要读取的内容数量

```
f = open('path1/小娃娃.txt',mode='r',encoding='utf-8')
msg = f.read(3)  #读取三个字符
msg1 = f.read()  #后边在读就会继续向后读取
f.close()
print(msg)
print(msg1)

结果: 
高圆圆

刘亦菲
张柏芝
杨紫
王菲
```

上边现在使用的是r模式这样读取的就是文字,如果使用rb模式读取出来的就是字节　　

```
f = open('path1/小娃娃.txt',mode='rb')
msg = f.read(3)
msg1 = f.read()
f.close()
print(msg)
print(msg1)

结果:
b'\xe9\xab\x98'
b'\xe5\x9c\x86\xe5\x9c\x86\r\n\xe5\x88\x98\xe4\xba\xa6\xe8\x8f\xb2\r\n\xe5\xbc\xa0\xe6\x9f\x8f\xe8\x8a\x9d\r\n\xe6\x9d\xa8\xe7\xb4\xab\r\n\xe7\x8e\x8b\xe8\x8f\xb2'
```

read()的弊端就是当文件很大的时候,将文件中的内容全部读取,存放在内存中这样会导致内存奔溃

### 3.2 readline()

readline()读取每次只读取一行,注意点:readline()读取出来的数据在后面都有一个\n

```
f = open('path1/小娃娃.txt',mode='r',encoding='utf-8')
msg1 = f.readline()
msg2 = f.readline()
msg3 = f.readline()
msg4 = f.readline()
f.close()
print(msg1)
print(msg2)
print(msg3)
print(msg4)

结果:
高圆圆

刘亦菲

张柏芝

杨紫
```

结果这里每个一内容中间都有一个空行是因为咱们读取的内容后边都带有一个\n print()的时候会将这个\n执行

```
f = open('path1/小娃娃.txt',mode='r',encoding='utf-8')
msg1 = f.readline().strip()
msg2 = f.readline().strip()
msg3 = f.readline().strip()
msg4 = f.readline().strip()
f.close()
print(msg1)
print(msg2)
print(msg3)
print(msg4)

结果:
高圆圆
刘亦菲
张柏芝
杨紫
```

解决这个问题只需要在我们读取出来的文件后边加一个strip()就OK了

### 3.3 readlines()

readlines() 将每一行形成一个元素，放到一个列表中，将所以的内容全部读出来，如果文件很大，占内存，容易崩盘。

```
f = open('log',encoding='utf-8')
print(f.readlines())
f.close()
# 结果['666666\n', 'fkja l;\n', 'fdkslfaj\n', 'dfsflj\n', 'df;asdlf\n', '\n', ]
```

如果有个较大的文件我们进行读取不推荐使用以下方法:

```
f = open('../path1/弟子规',mode='r',encoding='utf-8')  # 此处的../是返回到父目录
print(f.read()) #这样就是将文件一次性全部读取到内存中,内存容易奔溃
```

推荐使用的是这种方法:

```
f = open('../path1/弟子规',mode='r',encoding='utf-8')
for line in f:
    print(line)      #这种方式就是在一行一行的进行读取,它就执行了下边的功能

print(f.readline())
print(f.readline())
print(f.readline())
print(f.readline())
f.close()
```

注意点:读完的文件句柄一定要关闭　

## 四.写模式

### 4.1 覆盖写

在写文件的时候我们要养成一个写完文件就刷新的习惯.  刷新flush()

```
f = open('../path1/小娃娃.txt',mode='w',encoding='utf-8')
f.write('太白很白')
f.flush()
f.close()

结果:
当我选择使用w模式的时候,在打开文件的时候就就会把文件中的所有内容都清空,然后在操作
```

![](assets\1-1548382959580.gif)

注意点:如果文件不存在使用w模式会创建文件,文件存在w模式是覆盖写,在打开文件时会把文件中所有的内容清空. 

```
f1 = open('../path1/小娃娃.txt',mode='r',encoding='utf-8')
msg = f1.read()
print(msg)

# 这个是先查看小娃娃文件中有哪些内容


f = open('../path1/小娃娃.txt',mode='w',encoding='utf-8')
f.write('太白很白')
f.flush()
f.close()
# 这个是对小娃娃文件进行覆盖写操作


f1 = open('../path1/小娃娃.txt',mode='r',encoding='utf-8')
msg = f1.read()
print(msg)

# 查看覆盖写后的内容
```

尝试读一读

```
f1 = open('../path1/小娃娃.txt',mode='w',encoding='utf-8')
msg = f1.read()
print(msg)

结果:
Traceback (most recent call last):
  File "D:/python_object/path2/test.py", line 563, in <module>
    msg = f1.read() 
io.UnsupportedOperation: not readable    #模式是w,不可以执行读操作
```

wb模式下,不可以指定打开文件的编辑,但是写文件的时候必须将字符串转换成utf-8的bytes数据

```
f = open('../path1/小娃娃.txt',mode='wb')
msg = '你好'.encode('utf-8')
f.write(msg)
f.flush()  # 刷新
f.close()
```

### 4.2 追加

只要是a或者ab,a+都是在文件的末尾写入,不论光标在任何位置.

在追加模式下,我们写入的内容后追加在文件的末尾

a模式如果文件不存在就会创建一个新文件

```
f1 = open('../path1/小娃娃.txt',mode='a',encoding='utf-8')
msg = f1.write('这支烟灭了以后')
```

ab这个模式,自己试一下 没有什么太大的差别

## 五.读写模式

对于读写模式,必须是先读后写,因为光标默认在开头位置,当读完了以后再进行写入.我们以后使用频率最高的模式就是r+

### 5.1 r+模式

看下正确的操作:

```
f1 = open('../path1/小娃娃.txt',mode='r+',encoding='utf-8')
msg = f1.read()
f1.write('这支烟灭了以后')
f1.flush()
f1.close()
print(msg)
结果:
正常的读取之后,写在结尾
　
```

看下错误的操作:

```
f1 = open('../path1/小娃娃.txt',mode='r+',encoding='utf-8')
f1.write('小鬼')
msg = f1.read()
f1.flush()
f1.close()
print(msg)

结果:
这样写会把小鬼写在开头,并且读出来的是小鬼之后的内容
```

r+模式一定要记住是先读后写

### 5.2 r+模式坑

深坑请注意: 在r+模式下. 如果读取了内容. 不论读取内容多少. 光标显示的是多少. 再写入
或者操作文件的时候都是在结尾进行的操作.

## 六.写读模式

先将所有的内容清空,然后写入.最后读取.但是读取的内容是空的,不常用

```
f1 = open('../path1/小娃娃.txt',mode='w+',encoding='utf-8')
f1.write('小鬼')
msg = f1.read()
f1.flush()
f1.close()
print(msg)　　
```

有人说,先读在写不就行了.w+模式下 其实和w模式一样,把文件清空了,在写的内容.所以很少人用

### 追加读(a+,a+b)

 a+模式下,不论是先读还是后读,都是读不到数据的

```
f = open('../path1/小娃娃.txt',mode='a+',encoding='utf-8')
f.write('阿刁')
f.flush()
msg = f.read()
f.close()
print(msg)
```

还有几个带b的模式,其实就是对字节的一些操作,就不多叙述了.

## 七.其他相关操作　

### 7.1 seek()

seek(n)光标移动到n位置,注意: 移动单位是byte,所有如果是utf-8的中文部分要是3的倍数

通常我们使用seek都是移动到开头或者结尾

移动到开头:seek(0,0)  可以看做成seek(0)

seek(6)这种如果是单数并且不是0的就是按照字节来移动光标

移动到结尾:seek(0,2) seek的第二个参数表示的是从哪个位置进行偏移,默认是0,表示开头,1表示当前位置,2表示结尾

```
f = open("小娃娃", mode="r+", encoding="utf-8")
f.seek(0) # 光标移动到开头
content = f.read() # 读取内容, 此时光标移动到结尾
print(content)
f.seek(0) # 再次将光标移动到开头
f.seek(0, 2) # 将光标移动到结尾
content2 = f.read() # 读取内容. 什么都没有
print(content2)
f.seek(0) # 移动到开头
f.write("张国荣") # 写入信息. 此时光标在9 中文3 * 3个 = 9
f.flush()
f.close()　
tell()
```

### 7.2 tell()

使用tell()可以帮我们获取当前光标在什么位置

```
f = open("小娃娃", mode="r+", encoding="utf-8")
f.seek(0) # 光标移动到开头
content = f.read() # 读取内容, 此时光标移动到结尾
print(content)
f.seek(0) # 再次将光标移动到开头
f.seek(0, 2) # 将光标移动到结尾
content2 = f.read() # 读取内容. 什么都没有
print(content2)
f.seek(0) # 移动到开头
f.write("张国荣") # 写入信息. 此时光标在9 中⽂文3 * 3个 = 9
print(f.tell()) # 光标位置9
f.flush()
f.close()
```

### 7.3 修改文件

文件修改: 只能将文件中的内容读取到内存中, 将信息修改完毕, 然后将源文件删除, 将新文件的名字改成老文件的名字.

```
import os
with open("../path1/小娃娃", mode="r", encoding="utf-8") as f1,\
open("../path1/小娃娃_new", mode="w", encoding="UTF-8") as f2:
    content = f1.read()
    new_content = content.replace("冰糖葫芦", "⼤白梨")
    f2.write(new_content)
os.remove("../path1/小娃娃") # 删除源文件
os.rename("../path1/小娃娃_new", "小娃娃") # 重命名新文件
```

弊端: ⼀次将所有内容进行读取. 内存溢出. 解决方案: 一行一行的读取和操作

```
import os
with open("小娃娃", mode="r", encoding="utf-8") as f1,\
open("小娃娃_new", mode="w", encoding="UTF-8") as f2:
    for line in f1:
        new_line = line.replace("大白梨", "冰糖葫芦")
        f2.write(new_line)
os.remove("小娃娃") # 删除源⽂文件
os.rename("小娃娃_new", "小娃娃") # 重命名新文件
```

