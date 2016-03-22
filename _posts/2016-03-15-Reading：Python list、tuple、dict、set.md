---
layout: post
title:  "Reading：Python list、tuple、dict、set"
date:   2016-03-15 09:00:13
categories: [Python, Reading]
permalink: /archivers/reading-python-list-tuple-dict-set
---
本文为读书笔记，书籍为《Head First Python》、《Beginning Python: From Novice to Professional》、《Core Python Programming》。

# 序列

## 操作符

- 连接操作符

连接操作符的性能优化

```python
>>> sayhi = 'hello '+'world '+'!'	#Python必须为每一个参加连接操作的字符串分配新的内存，包括新产生的字符串
>>> sayhi
'hello world !'
>>> sayhi = ''.join(['hello ','world ','!'])	#将子字符串放到一个可迭代的对象中(列表、元组等)
>>> sayhi
'hello world !'
>>> list1 = [1,2,3]
>>> list2 = [4,5,6]
>>> print(list1+list2)
[1, 2, 3, 4, 5, 6]
>>> list1.extend(list2)		#类似地，列表的合并推荐使用extend()
>>> print(list1)
[1, 2, 3, 4, 5, 6]
```

## 格式化操作符

```python
>>> print('hi %s,welcome!'%('chen'))	#优先使用str()函数进行字符串转换
hi chen,welcome!
```

##字符串

Pyhon里没有字符这个类型，而是用长度为1的字符串来表示这个概念。

字符串是一个不可变数据类型

```python
>>> s = 'hello'
>>> s[1]='i'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object does not support item assignment
```

## 字符串内建方法

```python
>>> 'helloworld'.count('o')		#string.count(str,beg=0,end=len(string))，返回计数
2
>>> 'helloworld'.find('o')	#string.find(str,beg=0,end=len(string)),返回索引值
4
>>> 'helloworld'.split('o')	#string.split(str,num=string.count(str))，默认分隔个数为全部，返回list
['hell', 'w', 'rld']
```

# 列表 List

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

#测试
>>> list1 = [1,2,3]
>>> list1.extend(('hello','world')) #同样可以添加元组
>>> list1
[1, 2, 3, 'hello', 'world']
```

- 删除

```python
cast.pop()      #会返回被删除的对象
cast.pop(1) 	#可以指定索引位置
cast.remove('chapman')
del mylist	#删除一个列表
del mylist[1]	#删除列表索引为1的元素
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
>>> str1 = '1234567890'
>>> str1[::-1]	#翻转
'0987654321'
>>> str1[::-2]	#翻转后，step为2
'08642'
>>> str1[-100:100]	#越界不报错
'1234567890'
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

#若range前两个参数为递增/递减，则第三个参数必为正/负
#range(-1,-5,-1) 正确：-1,-2,-3,-4
#range(-1,-5,1) 错误
#range(-5,-1,1)	正确 -5,-4,-3,-2
#range(1,5,1) 正确 1,2,3,4

```

## 列表注意事项

- 列表是可变数据类型

```python
>>> list1 = [1,2,3]
>>> list2 = [1,2,3]
>>> id(list1)
2051551280968
>>> id(list2)	#列表是可变数据类型
2051551280200
>>> id(list1[1])
1739517776
>>> id(list2[1])	#整型是不可变数据类型
1739517776
```


# 元组 tuple

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

- tuple的关键不在于(),而在于,

```python
>>> tuple1 = 1,2,3
>>> tuple1
(1, 2, 3)
>>> type(tuple1)
<class 'tuple'>
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

# 字典 dict

dict全称dictionary，在其他语言中也称为map，使用键-值（key-value）存储，具有极快的查找速度。**特点：查找速度快、无序、key不可变。**

## 字典创建

```python
>>> dict1 = dict({('a',1),('b',2),('c',3)})	#dict()内可以是列表或元组
>>> dict1
{'c': 3, 'b': 2, 'a': 1}
>>> dict2 = dict(a='first',b='second',c='third')
>>> dict2
{'c': 'third', 'b': 'second', 'a': 'first'}
>>> dict3 = {}
>>> dict3.fromkeys((1,2,3))	#使用fromkeys()之前需要定义dict3
{1: None, 2: None, 3: None}
>>> dict3.fromkeys((1,2,3),'hello')
{1: 'hello', 2: 'hello', 3: 'hello'}
>>> dict3.fromkeys((1,2,3),('hello','world'))
{1: ('hello', 'world'), 2: ('hello', 'world'), 3: ('hello', 'world')}
>>> dict3
{}
>>> dict3 = dict3.fromkeys((1,2,3),('hello','world'))
>>> dict3
{1: ('hello', 'world'), 2: ('hello', 'world'), 3: ('hello', 'world')}

```

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

## 字典循环

```python
>>> dict1 = {}
>>> dict1 = dict1.fromkeys(range(6),'hello')
>>> for eachkey in dict1.keys():
...     print(eachkey)
...
0
1
2
>>> for eachvalue in dict1.values():
...     print(eachvalue)
...
hello
hello
hello
>>> for eachitem in dict1.items():
...     print(eachitem)
...
(0, 'hello')
(1, 'hello')
(2, 'hello')
```

## 浅拷贝

```python
>>> a = {'a':1,'b':2,'c':3}
>>> b = a
>>> c = a.copy()	#浅拷贝，相当于重新创建一个字典
>>> id(a)
2545670190280
>>> id(b)
2545670190280
>>> id(c)
2545670190472
>>> a['d'] = 4
>>> b
{'a': 1, 'b': 2, 'c': 3, 'd': 4}
>>> c
{'a': 1, 'b': 2, 'c': 3}
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

# 集合 set

set的内部结构和dict很像，唯一区别是不存储value。set的元素**不重复**，而且是**无序**的。

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
>>> id(s)
2545670177288
>>> s.add('c')	#添加元素
>>> id(s)
2545670177288
```

## 不可变集合

```python
>>> s = frozenset([1,2,3])
>>> s
frozenset({1, 2, 3})
>>> s.add(4)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: 'frozenset' object has no attribute 'add'
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

#三元表达式

```python
>>> x=1
>>> y=2
>>> num = x if x<y else y
>>> num
1
```

#断言assert

```python
>>> assert 3>4
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AssertionError
```

# 条件判断

# 循环

## for循环

# 递归

- 斐波那契数列

斐波那契数列又称黄金分割数列、因数学家列昂纳多·斐波那契（Leonardoda Fibonacci ）以兔子繁殖为例子而引入，故又称为“兔子数列”（一个数总是等于前两个数字之和）。

```python
#斐波那契数列：1、1、2、3、5、8、13、21、34
def fibonacci(num):
	if num in (1,2):
		return 1
	else:
		return fibonacci(num-1)+fibonacci(num-2)

print(fibonacci(7))

#输出13
```

#break和continue

- break 跳出循环体

```python
for each in range(1,6):
	if each%3 == 0:
		print('第一个能被3整除的数已找到：'+str(each))
		break
	print(str(each)+'不可以被3整除')
    
1不可以被3整除
2不可以被3整除
第一个能被3整除的数已找到：3
```

- continue 终止本轮循环，开始下一轮循环

```python
for each in range(6):
	if each%2 == 0:
		print(each)
		continue
	print('这是一个奇数')

0
这是一个奇数
2
这是一个奇数
4
这是一个奇数
```



## while循环

# 操作符