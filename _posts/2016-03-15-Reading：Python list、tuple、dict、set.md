---
layout: post
title:  "Reading：Python list、tuple、dict、set"
date:   2016-03-15 09:00:13
categories: [Python, Reading]
permalink: /archivers/reading-python-list-tuple-dict-set
---
本文为读书笔记，书籍为《Head First Python》、《Beginning Python: From Novice to Professional》、《Core Python Programming》。

#  列表、元组、字典、集合

![](http://github.com/thegofind/thegofind.github.io/raw/master/img/00008.png)

# 序列

Python中的序列包括list、tuple、str、range，如没有特别指明，本文关于序列的所有介绍，都适用于list、tuple、str、range。（同list、tuple、str一样，range本身也是一种类型）

## 拼接

- 使用 + 操作符拼接序列，必须保证 + 左右两边的数据类型相同。( + 操作符无法拼接非序列对象)
- 使用 + 操作符拼接并不高效，Python必须为每一个参加连接操作的序列分配新的内存，包括新产生的序列。
- 拼接字符串str时，join()方法可以更高效地完成拼接。join()方法可以拼接任何可迭代对象，包括dict和set。
- 拼接列表list时，extend()方法可以更高效地完成拼接。extend()方法可以拼接任何可迭代对象，包括dict和set。

```python
#字符串拼接
>>> sayhi = 'h'+'i' #Python必须为每一个参加连接操作的字符串分配新的内存，包括新产生的字符串
>>> sayhi = ''.join(['h','i'])	#将子字符串放到一个可迭代的对象中(list、tuple、str、dict、set)
>>> sayhi = '-'.join({'a','b','c'})	#可迭代的对象包括dict和set，但由于为无序数据，因此并不常用
>>> sayhi
'c-a-b'
>>> sayhi = '-'.join({'a':'1','b':'2','c':'3'})	#可迭代对象必须为字符串，因此range虽可迭代，但并不适用
>>> sayhi
'c-a-b'

#列表拼接
>>> list1 = [1,2,3]
>>> list2 = [4,5,6]
>>> list1.extend(list2)	
```


## 切片

- sequence[ star : end : step ]

序列（包括list、tuple、str、range）都可以使用切片，非序列对象（包括dict、set）不存在顺序，因此无法使用切片。

```python
>>> sequence[-3:]   #取最后三个元素
>>> sequence[0:3]   #不包括list[3]
>>> sequence[:0]  #为空序列
>>> sequence[::-2]	#翻转后，step为2
>>> sequence[-100:100]	#越界不报错
```

##  序列的可变性

```python
>>> list1 = [1,1.1,'a']
>>> list2 = [1,1.1,'a']
>>> id(list1)
2545670176584
>>> id(list2)	#为什么同样内容的列表不是同一个引用？因为列表是可变数据类型。
2545670176456
>>> id(list1[0])
1735257904
>>> id(list2[0])	#整型是不可变数据类型
1735257904
>>> id(list1[1])
2545643921696
>>> id(list2[1])	#浮点数是可变数据类型
2545643921648
>>> id(list1[2])
2545669600400
>>> id(list2[2])	#字符串(包括字符)是不可变数据类型
2545669600400
```

# 序列之：字符串 str

Pyhon里没有字符这个类型，而是用长度为1的字符串来表示这个概念。

字符串是一个**不可变数据类型**。

```python
>>> s = 'hello'
>>> s[1]='i'
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object does not support item assignment
```

## 内建函数

- count()、find()、split()、strip()


```python
>>> 'helloworld'.count('o')		#string.count(str,beg=0,end=len(string))，返回计数
2
>>> 'helloworld'.find('o')	#string.find(str,beg=0,end=len(string))，返回索引值
4
>>> 'helloworld'.split('l',2)	#string.split(str,deep)，默认深度为最大深度，返回list
['he', '', 'oworld']
>>> ' hello world '.strip()	#去除首尾空格，保留中间的空格
'hello world'
```

# 序列之：列表  list

list是一个可变的**有序**表，其中可以存储**任意类型的数据**（包括混合类型），可称之为‘打了激素的数组’。

## 内建函数

- list.count(obj)、list.index(obj,start,stop)、list.sort(func=None,key=None,reverse=False)


# 序列之：元组 tuple

tuple和list非常类似，但是tuple一旦初始化就不能修改，它也没有append()，insert()这样的方法。获取元素的方法和list是一样的，但不能赋值成另外的元素。

## 元组注意事项

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

# 映射之：字典 dict

dict全称dictionary，在其他语言中也称为map，使用键-值（key-value）存储，具有极快的查找速度。**特点：查找速度快、无序、key不可变。**

使用工厂函数dict()创建字典时，参数必须是可迭代的，且每个可迭代的元素必须成对出现。

## 内建函数

- clear()、pop()、fromkeys()、get()、keys()、values()、items()、update()

## 字典特性

- dict的key必须是不可变对象

dict根据key来计算value的存储位置，如果每次计算相同的key得出的结果不同，那dict内部就完全混乱了。这个通过key计算位置的算法称为哈希算法（Hash）。

在Python中，字符串、数字等都是可哈希的，因此，可以放心地作为key。而list是可变的，就不能作为key。

- 为什么dict查找速度这么快？

因为dict的实现原理和查字典是一样的。先在字典的索引表里（比如部首表）查这个字对应的页码，然后直接翻到该页，找到这个字。

- 和list比较，dict有以下几个特点

查找和插入的速度极快，不会随着key的增加而变慢；
需要占用大量的内存，内存浪费多。

- 而list相反：

查找和插入的时间随着元素的增加而增加；
占用空间小，浪费内存很少。

# 集合之：集合 set

set的内部结构和dict很像，唯一区别是不存储value。set的元素**不重复**，而且是**无序**的。

## 集合操作符

```python
# == 和 !=
>>> set('shop') == set('posh')
True
# | $ -
>>> s1 = set('cheeseshop')
>>> s2 = set('bookshop')
>>> s1|s2
{'c', 'p', 'h', 'e', 'o', 'k', 's', 'b'}
>>> s1&s2
{'h', 'p', 'o', 's'}
>>> s1-s2
{'c', 'e'}
```

## 集合的应用

```python
#set最经典的示例就是罗列星期表，因为星期表是固定不变的
weekdays = set(['MON', 'TUE', 'WED', 'THU', 'FRI', 'SAT', 'SUN'])
if 'MON' in weekdays:   #查找某元素是否存在于该set
	print('MON is existing')
```

# 可迭代

可迭代的数据类型包括：str、list、tuple、range、dict、set。

## 循环

```python
for each in sequence:
        print(each)
```

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

### 格式化操作符

```python
>>> print('hi %s,welcome!'%('chen'))	#优先使用str()函数进行字符串转换
hi chen,welcome!
```

![](http://github.com/thegofind/thegofind.github.io/raw/master/img/00009.png)