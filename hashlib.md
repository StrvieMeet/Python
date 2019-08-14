### **一. hashlib模块**

​    此模块有人称为摘要算法，也叫做加密算法，或者是哈希算法，散列算法等等，这么多title不用大家记，那么有同学就问他到底是干啥的？ 简单来说就是做加密和校验使用，它的工作原理给大家简单描述一下：它通过一个函数，把任意长度的数据按照一定规则转换为一个固定长度的数据串（通常用16进制的字符串表示）。

比如：之前我们在一个文件中存储用户的用户名和密码是这样的形式：

​    宝元|123456

有什么问题？你的密码是明文的，如果有人可以窃取到这个文件，那么你的密码就会泄露了。所以，一般我们存储密码时都是以密文存储，比如：

​    宝元|4665ace0eb5d3d6a2822a7c455587e47

那么即使是他窃取到这个文件，他也不会轻易的破解出你的密码，这样就会保证了数据的安全。

hashlib模块就可以完成的就是这个功能。

hashlib的特征以及使用要点：

1. bytes类型数据 ---> 通过hashlib算法 ---> 固定长度的字符串
2. 不同的bytes类型数据转化成的结果一定不同。
3. 相同的bytes类型数据转化成的结果一定相同。
4. 此转化过程不可逆。

那么刚才我们也说了，hashlib的主要用途有两个：

​    **密码的加密。**

​    **文件一致性校验。**

hashlib模块就相当于一个算法的集合，这里面包含着很多的算法，算法越高，转化成的结果越复杂，安全程度越高，相应的效率就会越低。

**普通加密：**

我们以常见的摘要算法MD5为例，计算出一个字符串的MD5值：

```
import hashlib

md5 = hashlib.md5()
md5.update('123456'.encode('utf-8')) # 必须是bytes类型才能够进行加密
print(md5.hexdigest())

# 计算结果如下：
'e10adc3949ba59abbe56e057f20f883e'

# 验证：相同的bytes数据转化的结果一定相同

import hashlib
md5 = hashlib.md5()
md5.update('123456'.encode('utf-8'))
print(md5.hexdigest())

# 计算结果如下：
'e10adc3949ba59abbe56e057f20f883e'

# 验证：不相同的bytes数据转化的结果一定不相同
import hashlib

md5 = hashlib.md5()
md5.update('12345'.encode('utf-8'))
print(md5.hexdigest())

# 计算结果如下：
'827ccb0eea8a706c4c34a16891f84e7b'
```

上面就是普通的md5加密，非常简单，几行代码就可以了，但是这种加密级别是最低的，相对来说不很安全。虽然说hashlib加密是不可逆的加密方式，但也是可以破解的，那么他是如何做的呢？你看网上好多MD5解密软件，他们使用撞库的方式。他们会把常用的一些密码比如：123456,111111,以及他们的md5的值做成对应关系，类似于字典，

dic = {'e10adc3949ba59abbe56e057f20f883e': 123456}

循环他们那定义的字典中的键和咱们生成的密文进行比较,比较成功后通过你的密文获取对应的密码。

所以针对刚才说的情况，我们有更安全的加密方式：加盐。

**加盐加密**

​    **固定的盐**

什么叫加盐？加盐这个词儿来自于国外，外国人起名字我认为很随意，这个名字来源于烧烤，俗称BBQ。我们烧烤的时候，一般在快熟的时候，都会给肉串上面撒盐，增加味道，那么这个撒盐的工序，外国人认为比较复杂，所以就将比较复杂的加密方式称之为加盐。

其实代码非常简单：

```
ret = hashlib.md5('xx教育'.encode('utf-8'))  # xx教育就是固定的盐
ret.update('a'.encode('utf-8'))
print(ret.hexdigest())
```

上面的xx教育就是固定的盐，比如你在一家公司，公司会将你们所有的密码在md5之前增加一个固定的盐，这样提高了密码的安全性。但是如果黑客通过手段窃取到你这个固定的盐之后，也是可以破解出来的。所以，我们还可以加动态的盐。

​    **动态的盐**

```python
username = '宝元666'
ret = hashlib.md5(username[::2].encode('utf-8'))  # 针对于每个账户，每个账户的盐都不一样
ret.update('a'.encode('utf-8'))
print(ret.hexdigest())
```

这样，安全性能就大大提高了。

那么我们之前说了hahslib模块是一个算法集合，他里面包含很多种加密算法，刚才我们说的MD5算法是比较常用的一种加密算法，一般的企业用MD5就够用了。但是对安全要求比较高的企业，比如金融行业，MD5加密的方式就不够了，得需要加密方式更高的，比如sha系列，sha1,sha224,sha512等等，数字越大，加密的方法越复杂，安全性越高，但是效率就会越慢。

```python
ret = hashlib.sha1()
ret.update('guobaoyuan'.encode('utf-8'))
print(ret.hexdigest())

#也可加盐
ret = hashlib.sha384(b'asfdsa')
ret.update('guobaoyuan'.encode('utf-8'))
print(ret.hexdigest())

# 也可以加动态的盐
ret = hashlib.sha384(b'asfdsa'[::2])
ret.update('guobaoyuan'.encode('utf-8'))
print(ret.hexdigest())
```

不过一般我们用到MD5加密就可以了。

**4.42 文件的一致性校验**

hashlib模块除了可以用于密码加密之外，还有一个常用的功能，那就是文件的一致性校验。

​    linux讲究：一切皆文件，我们普通的文件，是文件，视频，音频，图片，以及应用程序等都是文件。我们都从网上下载过资源，比如我们刚开学时让大家从网上下载Python解释器，当时你可能没有注意过，其实你下载的时候都是带一个MD5或者shax值的，为什么？ 我们的网络世界是很不安全的，经常会遇到病毒，木马等，有些你是看不到的可能就植入了你的电脑中，那么他们是怎么来的？ 都是通过网络传入来的，就是你在网上下载一些资源的时候，趁虚而入，当然大部分被我们的浏览器或者杀毒软件拦截了，但是还有一部分偷偷的进入你的磁盘中了。那么我们自己如何验证我们下载的资源是否有病毒呢？这就需要文件的一致性校验了。在我们下载一个软件时，往往都带有一个MD5或者shax值，当我们下载完成这个应用程序时你要是对比大小根本看不出什么问题，你应该对比他们的md5值，如果两个md5值相同，就证明这个应用程序是安全的，如果你下载的这个文件的MD5值与服务端给你提供的不同，那么就证明你这个应用程序肯定是植入病毒了（文件损坏的几率很低），那么你就应该赶紧删除，不应该安装此应用程序。

我们之前说过，md5计算的就是bytes类型的数据的转换值，同一个bytes数据用同样的加密方式转化成的结果一定相同，如果不同的bytes数据（即使一个数据只是删除了一个空格）那么用同样的加密方式转化成的结果一定是不同的。所以，hashlib也是验证文件一致性的重要工具。

![image-20190714141603605](assets/image-20190714141603605.png)

我们在安装python解释器的时候,在安装python解释器的时候计算本地的md5值是否一致,一致安装,不一致的删除.

我将文件校验写在一个函数中

**low版文件校验：**

```python
def func(file):
    with open(file,mode='rb') as f1:
        ret = hashlib.md5()
        ret.update(f1.read())
        return ret.hexdigest()

print(func('hashlib_file1'))
```

这样就可以计算此文件的MD5值，从而进行文件校验。但是这样写有一个问题，有什么问题？如果文件过大，全部读取出来直接就会撑爆内存的，所以我们要分段读取，那么分段读取怎么做呢？

hashlib还可以这样玩：

```python
import hashlib
# 直接 update
md5obj = hashlib.md5()
md5obj.update('宝元 is a old driver'.encode('utf-8'))
print(md5obj.hexdigest())  # da525c66739e6baa8729332f8bae8e0f

# 分段update
md5obj = hashlib.md5()
md5obj.update('宝元 '.encode('utf-8'))
md5obj.update('is '.encode('utf-8'))
md5obj.update('a '.encode('utf-8'))
md5obj.update('old '.encode('utf-8'))
md5obj.update('driver'.encode('utf-8'))
print(md5obj.hexdigest())  # da525c66739e6baa8729332f8bae8e0f
# 结果相同
```

我们现在知道可以进行分段update后,我们就可以迭代的获取文件中的内容,现在来做一个高大上版文件校验

**高大上版文件校验**

**校验Pyhton解释器的Md5值是否相同**

```python
import hashlib

def file_check(file_path):
    with open(file_path,mode='rb') as f1:
        sha256 = hashlib.md5()
        while 1:
            content = f1.read(1024)
            if content:
                sha256.update(content)
            else:
                return sha256.hexdigest()
print(file_check('python-3.6.6-amd64.exe'))
```

![2](assets/2.gif)

### 