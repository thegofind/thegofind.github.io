---
layout: post
title:  "Reading：Python list、tuple、dict、set"
date:   2016-03-15 09:00:13
categories: [Python, Reading]
permalink: /archivers/reading-python-list-tuple-dict-set
---
本文为读书笔记，书籍为《Head First Python》、《Beginning Python: From Novice to Professional》、《Core Python Programming》。

# 列表List

list是一个可变的**有序**表，其中可以存储**任意类型的数据**（包括混合类型），可称之为‘打了激素的数组’。

## 列表操作

cast = ['cleese','palin','jones','idle']

- 长度

```python
    len(cast)
```

- 取值/修改

```python
    cast[1]
    cast[-1]    
    cast[1] = 'chapman'
```

- 插入

```python
    cast.append('gilliam')
    cast.insert(0,'chapman')
    cast.extend(['gilliam','chapman']) #添加另一个列表
```

- 删除

```python
    cast.pop()      #会返回被删除的对象
    cast.pop(1) 
    cast.remove('chapman')
```

- 数据类型

```python
    h = ['Apple', 123, True]
    n = ['python', 'java', ['asp', 'php'], 'scheme']  #长度为4，取'php'用n[2][1]
```

- 列表循环

```python
fav_movies = ['the holy grail','the life of brian']
    for each_flick in fav_movers:
        print(each_flick)
```

## 字符切片

```python
>>> name = 'Leonardo DiCaprio'  #字符串'xxx'和Unicode字符串 u'xxx'也可以看成是一种list，每个元素就是一个字符。
>>> name[-3:]   #取最后三个元素
'rio'
>>> list = ['Leonardo','DiCaprio','Kate','Winslet']
>>> list[-2:]
['Kate', 'Winslet']
>>> list = ['L','D','K','W','C','H','R']
>>> list[0:3]   #不包括list[3]
['L', 'D', 'K']
>>> list[0:-1:2]    #list[0:-1]间隔为2
['L', 'K', 'C']
>>> list[0:-1:3]
['L', 'W']
```

## 列表循环

- range()

```python
for each_item in range(1,15,3):   #range()的使用方式和字符切片十分相似
	print(each_item)

PS C:\whatever\python> python test.py   
1
4
7
10
13
```

## 列表注意事项


# 元组tuple

tuple和list非常类似，但是tuple一旦初始化就不能修改，它也没有append()，insert()这样的方法。获取元素的方法和list是一样的，但不能赋值成另外的元素。

- 不可变的tuple有什么意义？

因为tuple不可变，所以代码更安全。如果可能，能用tuple代替list就尽量用tuple。

- 如何定义一个只有1个元素的tuple？

```python
>>> list = (1)  #这里的()被系统识别为数学公式中的小括号
>>> list[0]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'int' object is not subscriptable
>>> list = (1,)  #当元组只有一个元素时，可以添加一个,消除歧义
>>> list[0]
1
```

- 混合类型数据

```python
>>> t = ('a', 'b', ['A', 'B'])
>>> t[2][0] = 'X'
>>> t[2][1] = 'Y'
>>> t
('a', 'b', ['X', 'Y'])
```

![](http://github.com/thegofind/thegofind.github.io/raw/master/img/00003.png)

![](http://github.com/thegofind/thegofind.github.io/raw/master/img/00004.png)

表面上看，tuple的元素确实变了，但其实变的不是tuple的元素，而是list的元素。tuple一开始指向的list并没有改成别的list，所以，tuple所谓的“不变”是说，tuple的每个元素，指向永远不变。即指向'a'，就不能改成指向'b'，指向一个list，就不能改成指向其他对象，但指向的这个list本身是可变的！

# 字典dict

dict全称dictionary，在其他语言中也称为map，使用键-值（key-value）存储，具有极快的查找速度。**特点：查找速度快、无序、key不可变。**

## 字典操作

```python
>>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
>>> d
{'Bob': 75, 'Tracy': 85, 'Michael': 95}
>>> d.pop('Bob')    #删除元素
75
>>> d
{'Tracy': 85, 'Michael': 95}
>>> d['Tracy'] = 60    #赋值操作
>>> d
{'Tracy': 60, 'Michael': 95}
>>> d['Chen'] = 100    #赋值时，若key不存在，则添加相应的字典元素
>>> d
{'Tracy': 60, 'Michael': 95, 'Chen': 100}
>>> d.clear()   #清空字典
>>> d
{}
>>> d = {'a':1,'b':2,'c':3,'a':4}   #等同于对同一key多次赋值，取最后一次赋值
>>> d   
{'a': 4, 'c': 3, 'b': 2}
```

## 字典注意事项

- dict的key必须是不可变对象

dict根据key来计算value的存储位置，如果每次计算相同的key得出的结果不同，那dict内部就完全混乱了。这个通过key计算位置的算法称为哈希算法（Hash）。

在Python中，字符串、整数等都是不可变的，因此，可以放心地作为key。而list是可变的，就不能作为key。

- 为什么dict查找速度这么快？

因为dict的实现原理和查字典是一样的。先在字典的索引表里（比如部首表）查这个字对应的页码，然后直接翻到该页，找到这个字。

- 如何判断某个key是否存在？

```python
>>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85}
>>> 'Bob' in d  #方法1
True
>>> print(d.get('Tom'))  #方法2，当未找到key时返回第二个参数（可选）
None
>>> print(d.get('Tom',-1))
-1
```

- 字典dict的key-value顺序

```python
>>> d = {'Michael': 95, 'Bob': 75, 'Tracy': 85} 
>>> d           #字典key-value的顺序与放入顺序无关
{'Bob': 75, 'Tracy': 85, 'Michael': 95}
```

# set

set的内部结构和dict很像，唯一区别是不存储value。set的元素**不重复**，而且是**无序**的，且是**不可变**的。

## set操作

```python
>>> s = set(['a','b','c'])
>>> s.add('d')  #添加元素，若该元素已经存在，不会报错，会被忽视
>>> s
{'c', 'a', 'd', 'b'}
>>> s.remove('c')   #删除元素
>>> s
{'a', 'd', 'b'}
>>> s.remove('e')   #删除不存在的元素会报错
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
KeyError: 'e'
```

## set注意事项

```python
s = set(['a','b','c','d','e','f'])
for each_item in s:
	print(each_item)

PS C:\whatever\python> python test.py   #set是无序的
f
b
d
e
a
c
```

```python
#set最经典的示例就是罗列星期表，因为星期表是固定不变的
weekdays = set(['MON', 'TUE', 'WED', 'THU', 'FRI', 'SAT', 'SUN'])
if 'MON' in weekdays:   #查找某元素是否存在于该set
	print('MON is existing')
```

# list、tuple、dist、set 对比

| Tables  |  顺序  |  数据类型   | 是否可变 | 允许重复 |
| ------- | :--: | :-----: | :--: | :--: |
| 列表List  |  有序  | 复合数据类型  |  是   |  是   |
| 元组tuple |  有序  | 复合数据类型  |  否   |  是   |
| 字典dict  |  无序  | 非复合数据类型 |  是   | key否 |
| set     |  无序  |   key   |  是   |  否   |


- 和list比较，dict有以下几个特点

查找和插入的速度极快，不会随着key的增加而变慢；
需要占用大量的内存，内存浪费多。

- 而list相反：

查找和插入的时间随着元素的增加而增加；
占用空间小，浪费内存很少。

# 条件判断

# 循环

## for循环

- range()

## while循环

# 操作符