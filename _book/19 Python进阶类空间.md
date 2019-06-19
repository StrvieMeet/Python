# Python面向对象空间

## 一.Python 类的空间问题

### 1.1 何处可以添加对象属性

```
class A:
    def __init__(self,name):
        self.name = name

    def func(self,sex):
        self.sex = sex
```

```
# 类外面可以：
obj = A('barry')
obj.age = 18
print(obj.__dict__)  # {'name': 'barry', 'age': 18}

# 类内部也可以：
obj = A('barry') # __init__方法可以。
obj.func('男')  # func 方法也可以。
```

**总结：对象的属性不仅可以在__init__里面添加，还可以在类的其他方法或者类的外面添加。**

### 1.2 何处可以添加类的静态属性

```
class A:
    def __init__(self,name):
        self.name = name

    def func(self,sex):
        self.sex = sex
    
    def func1(self):
        A.bbb = 'ccc'
       
# 类的外部可以添加

A.aaa = 'taibai'
print(A.__dict__)

# 类的内部也可以添加。

A.func1(111)
print(A.__dict__)
```

**总结：类的属性不仅可以在类内部添加，还可以在类的外部添加。**

### 1.3 对象如何找到类的属性

之前咱们都学习过，实例化一个对象，可以通过点的方式找到类中的属性，那么他为什么可以找到类中的属性呢？

通过图解说明：

![img](https://img2018.cnblogs.com/blog/988316/201901/988316-20190123165540346-1685822133.png)

对象查找属性的顺序：先从对象空间找  ------> 类空间找 ------> 父类空间找 ------->.....

类名查找属性的顺序：先从本类空间找 -------> 父类空间找--------> ........

上面的顺序都是单向不可逆，类名不可能找到对象的属性。

##  二. 类与类之间的关系

⼤千世界, 万物之间皆有规则和规律. 我们的类和对象是对⼤千世界中的所有事物进⾏归类. 那事物之间存在着相对应的关系. 类与类之间也同样如此. 在⾯向对象的世界中. 类与类中存在以下关系:

1. 依赖关系
2. 关联关系
3. 组合关系
4. 聚合关系
5. 实现关系
6. 继承关系(类的三大特性之一：继承。)

### 2.1 依赖关系

⾸先, 我们设计⼀个场景. 还是最初的那个例⼦. 要把⼤象装冰箱. 注意. 在这个场景中, 其实是存在了两种事物的. ⼀个是⼤象, ⼤象负责整个事件的掌控者, 还有⼀个是冰箱, 冰箱负责被⼤象操纵. 

⾸先, 写出两个类, ⼀个是⼤象类, ⼀个是冰箱类

```
class Elphant:
    def __init__(self, name):
        self.name = name

    def open(self):
        '''
        开⻔
        '''
        pass
    
    def close(self):
        '''
        关⻔
        '''
        pass


class Refrigerator:
    
    def open_door(self):
        print("冰箱⻔被打开了")
    
    def close_door(self):
        print("冰箱⻔被关上了")
```

　　冰箱的功能非常简单, 只要会开⻔, 关⻔就⾏了. 但是⼤象就没那么简单了. 想想. ⼤象开⻔和关⻔的时候是不是要先找个冰箱啊. 然后呢? 打开冰箱⻔. 是不是打开刚才找到的那个冰箱⻔. 然后装⾃⼰. 最后呢? 关冰箱⻔, 注意, 关的是刚才那个冰箱吧. 也就是说. 开⻔和关⻔⽤的是同⼀个冰箱. 并且. ⼤象有更换冰箱的权利, 想进那个冰箱就进那个冰箱. 这时, ⼤象类和冰箱类的关系并没有那么的紧密. 因为⼤象可以指定任何⼀个冰箱. 接下来. 我们把代码完善⼀下.动作发起的主体是大象。

```
class Elphant:
    def __init__(self, name):
        self.name = name

    def open(self,obj1):
        '''
        开⻔

        '''
        print('大象要开门了，默念三声，开')
        obj1.open_door()

    def close(self):
        '''
        关⻔
        '''
        print('大象要关门了，默念三声，关')


class Refrigerator:

    def open_door(self):
        print("冰箱⻔被打开了")

    def close_door(self):
        print("冰箱⻔被关上了")


elphant1 = Elphant('大象')
haier = Refrigerator()
elphant1.open(haier)
```

依赖关系：将一个类的对象或者类名传到另一个类的方法使用。此时, 我们说, ⼤象和冰箱之间就是依赖关系. 我⽤着你. 但是你不属于我. 这种关系是最弱的.比如. 公司和雇员之间. 对于正式员⼯, 肯定要签订劳动合同. 还得⼩⼼伺候着. 但是如果是兼职. 那⽆所谓. 需要了你就来. 不需要你就可以拜拜了. 这⾥的兼职(临时⼯) 就属于依赖关系.我⽤你. 但是你不属于我

### 2.2 关系

其实这三个在代码上写法是⼀样的. 但是, 从含义上是不⼀样的.

#### 2.2.1 关联关系.

两种事物必须是互相关联的. 但是在某些特殊情况下是可以更改和更换的.

#### 2.2.2 聚合关系

属于关联关系中的⼀种特例. 侧重点是xxx和xxx聚合成xxx. 各⾃有各⾃的声明周期. 比如电脑. 电脑⾥有CPU, 硬盘, 内存等等. 电脑挂了. CPU还是好的. 还是完整的个体

#### 2.2.3 组合关系.

属于关联关系中的⼀种特例. 写法上差不多. 组合关系比聚合还要紧密. 比如⼈的⼤脑, ⼼脏, 各个器官. 这些器官组合成⼀个⼈. 这时. ⼈如果挂了. 其他的东⻄也跟着挂了

先看关联关系：

这个最简单. 也是最常⽤的⼀种关系. 比如. ⼤家都有男女朋友. 男⼈关联着女朋友. 女⼈关联着男朋友. 这种关系可以是互相的, 也可以是单⽅⾯的.

定义类

```
class Boy:
    def __init__(self,name,girlFriend=None):
        self.name = name
        self.girlFriend = girlFriend

    def have_a_diner(self):
        if self.girlFriend:
            print('%s 和 %s 一起晚饭'%(self.name,self.girlFriend.name))
        else:
            print('单身狗，吃什么饭')


class Girl:
    def __init__(self,name):
        self.name = name
```

实例(创建)日天对象

```
b = Boy('日天')
b.have_a_diner() # 此时是单身狗

# 突然有一天，日天牛逼了
b.girlFriend = '如花'
b.have_a_diner()  #共进晚餐
```

实例(创建)wusir对象

```
# wusir 生下来就有女朋友 服不服
gg = Girl('小花')
bb = Boy('wusir', gg)
bb.have_a_diner()

# 结果嫌他有点娘，不硬，分了
bb.girlFriend = None
bb.have_a_diner()
```

注意 此时Boy和Girl两个类之间就是关联关系. 两个类的对象紧密练习着. 其中⼀个没有了. 另⼀个就孤单的不得了. 关联关系, 其实就是 我需要你. 你也属于我. 这就是关联关系. 像这样的关系有很多很多. 比如. 学校和老师之间的关系.

学校和老师示例:

```
# 老师属于学校，必须有学校才可以工作
class School:

    def __init__(self,name,address):
        self.name = name
        self.address = address


class Teacher:

    def __init__(self,name,school):
        self.name = name
        self.school = school

s1 = School('北京校区','美丽的沙河')
s2 = School('上海校区','上海迪士尼旁边')
s3 = School('深圳校区','南山区')

t1 = Teacher('武大',s1)
t2 = Teacher('海峰',s2)
t3 = Teacher('日天',s3)

print(t1.school.name)
print(t2.school.name)
print(t3.school.name)
```

但是学校也是依赖于老师的，所以老师学校应该互相依赖。

```
class School:

    def __init__(self,name,address):
        self.name = name
        self.address = address
        self.teacher_list = []

    def append_teacher(self,teacher):
        self.teacher_list.append(teacher)
        
class Teacher:

    def __init__(self,name,school):
        self.name = name
        self.school = school

s1 = School('北京校区','美丽的沙河')
s2 = School('上海校区','上海迪士尼旁边')
s3 = School('深圳校区','南山区')

t1 = Teacher('武大',s1)
t2 = Teacher('海峰',s2)
t3 = Teacher('日天',s3)

s1.append_teacher(t1)
s1.append_teacher(t2)
s1.append_teacher(t3)

# print(s1.teacher_list)
# for teacher in s1.teacher_list:
#     print(teacher.name)
```

好了. 这就是关联关系. 当我们在逻辑上出现了. 我需要你. 你还得属于我. 这种逻辑 就是关联关系. 那注意. 这种关系的紧密程度比上⾯的依赖关系要紧密的多. 为什么呢? 想想吧

至于组合关系和聚合关系，其实代码上差别不大，咱们就以组合举例：

**组合：将一个类的对象封装到另一个类的对象的属性中，就叫组合。**

咱们设计一个游戏人物类，让实例化几个对象让这几个游戏人物实现互殴的效果。

```
class Gamerole:
    def __init__(self,name,ad,hp):
        self.name = name
        self.ad = ad
        self.hp = hp
    def attack(self,p1):
        p1.hp -= self.ad
        print('%s攻击%s,%s掉了%s血,还剩%s血'%(self.name,p1.name,p1.name,self.ad,p1.hp))
gailun = Gamerole('盖伦',10,200)
yasuo= Gamerole('亚索',50,80)

#盖伦攻击亚索
gailun.attack(yasuo)
# 亚索攻击盖伦
yasuo.attack(盖伦)
```

但是这样互相攻击没有意思，一般游戏类的的对战方式要借助武器，武器是一个类，武器类包含的对象很多：刀枪棍剑斧钺钩叉等等，所以咱们要写一个武器类。

```
class Gamerole:
    def __init__(self,name,ad,hp):
        self.name = name
        self.ad = ad
        self.hp = hp
    def attack(self,p1):
        p1.hp -= self.ad
        print('%s攻击%s,%s掉了%s血,还剩%s血'%(self.name,p1.name,p1.name,self.ad,p1.hp))

class Weapon:
    def __init__(self,name,ad):
        self.name = name
        self.ad = ad
    def weapon_attack(self,p1,p2):
        p2.hp = p2.hp - self.ad - p1.ad
        print('%s 利用 %s 攻击了%s,%s还剩%s血' %(p1.name,self.name,p2.name,p2.name,p2.hp))
```

接下来借助武器攻击对方：

```
pillow = Weapon('绣花枕头',2)
pillow.weapon_attack(barry,panky)
# 但是上面这么做不好，利用武器攻击也是人类是动作的发起者，所以不能是pillow武器对象，而是人类利用武器攻击对方
```

所以，对代码进行修改。

```
class Gamerole:
    def __init__(self,name,ad,hp):
        self.name = name
        self.ad = ad
        self.hp = hp
    def attack(self,p1):
        p1.hp -= self.ad
        print('%s攻击%s,%s掉了%s血,还剩%s血'%(self.name,p1.name,p1.name,self.ad,p1.hp))
        
    def equip_weapon(self,wea):
        self.wea = wea  # 组合：给一个对象封装一个属性改属性是另一个类的对象
class Weapon:
    def __init__(self,name,ad):
        self.name = name
        self.ad = ad
    def weapon_attack(self,p1,p2):
        p2.hp = p2.hp - self.ad - p1.ad
        print('%s 利用 %s 攻击了%s,%s还剩%s血'
              %(p1.name,self.name,p2.name,p2.name,p2.hp))


# 实例化三个人物对象：
barry = Gamerole('太白',10,200)
panky = Gamerole('金莲',20,50)
pillow = Weapon('绣花枕头',2)

# 给人物装备武器对象。
barry.equip_weapon(pillow)

# 开始攻击
barry.wea.weapon_attack(barry,panky)
```

上面就是组合，只要是人物.equip_weapon这个方法，那么人物就封装了一个武器对象，再利用武器对象调用其类中的weapon_attack方法
