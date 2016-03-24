---
layout: post
title:  "Reading：Python 迭代器、生成器、range()"
date:   2016-03-22 09:00:13
categories: [Python, Reading]
permalink: /archivers/reading-python-range
---
本文为读书笔记，书籍为《Head First Python》、《Beginning Python: From Novice to Professional》、《Core Python Programming》。

# range()

class range(stop)
class range(start, stop[, step])

若stop-start为正，即递增时，step必须同为正，否则序列为空。

若stop-start为负，即递减时，step必须同为负，否则序列为空。

## range的 优势

> The advantage of the [`range`](https://docs.python.org/3/library/stdtypes.html#range) type over a regular [`list`](https://docs.python.org/3/library/stdtypes.html#list) or [`tuple`](https://docs.python.org/3/library/stdtypes.html#tuple) is that a [`range`](https://docs.python.org/3/library/stdtypes.html#range) object will always take the same (small) amount of memory, no matter the size of the range it represents (as it only stores the `start`, `stop` and `step` values, calculating individual items and subranges as needed).


> 同list或tuple相比，range类型有一个优势：无论range包含的数据有多少，range总是只消耗很小、恒定的内存。（因为它只保存起始位置、终止位置和步数，根据需要单独计算每个元素。
> ## 包容测试、 元素索引查找、 切片、迭代

```python
>>> r = range(1,10,2)
>>> list(r)
[1, 3, 5, 7, 9]
>>> 2 in r	#包容测试
False
>>> 2 in r
False
>>> r.index(3)	#元素索引查找
1
>>> r[0:2]	#切片
range(1, 5, 2)
>>> list(r[0:2])
[1, 3]
>>> r[-1]
9
```

## == 和 !=

range()可以被当作序列，使用 == 和 != 进行比较，如果它们表示相同的值序列，则被视为相等。

```#python
>>> range(0) == range(2,1,2)
True
>>> range(1,5,3) == range(1,6,3)
True
```

# sorted()、reversed(0)

sorted(iterable,key=None, reverse=False])

```python
>>> dict1 = {'a':(1,6),'b':(2,5),'A':(3,4)}
>>> sorted(dict1)	#等同于sorted(dict1.keys())
['A', 'a', 'b']
>>> sorted(dict1.values(),key = lambda x : x[0])	#这里匿名函数的作用是将每一个value作为一个序列进行索引取值比较
[(1, 6), (2, 5), (3, 4)]
>>> sorted(dict1.values(),key = lambda x : x[1])
[(3, 4), (2, 5), (1, 6)]
```

sorted()和zip()返回一个序列（列表），reversed()和enumerate()返回迭代器（类似序列）。

# 函数何时有返回值？

在使用可变对象的方法如sort()、extend()和reverse()的时候要注意，这些操作会在可变对象中原地执行操作，也就说现有的可变对象的数据会被改变，但是没有返回值。

```python
>>> list1 = [1,2,3]
>>> list1.extend([4,5,6])
>>> list1
[1, 2, 3, 4, 5, 6]
>>> list2 = list1.extend([4,5,6])	#list1.extend()未显式设置返回值，即默认返回None
>>> list2
>>> type(list2)
<class 'NoneType'>
```

而不可变对象的方法是不能改变它们的值，所以它们必须返回一个新的对象。如果确实需要返回一个对象，可以使用如sorted()、reverse()等内建函数。

# 深拷贝、浅拷贝

浅拷贝：对不可变对象进行显式拷贝，并创建一个对象；对可变对象进行引用拷贝。

浅拷贝包括：

1、切片

2、工厂函数（如list()、dict()等)

3、copy模块的copy()函数

4、list、dict、set的内建函数copy()

```python
>>> import copy
>>> list1 = ['name',['price',400]]
>>> list2 = list1[:]
>>> list3 = list(list1)
>>> list4 = copy.copy(list1)
>>> list5 = copy.deepcopy(list1)
>>> id(list1[1])	
1439195035848
>>> id(list2[1])
1439195035848
>>> id(list3[1])
1439195035848
>>> id(list4[1])
1439195035848
>>> id(list5[1])	#deepcopy()是深拷贝
1439195035784
>>> list1[1][1] = 100
>>> list1
['name', ['price', 100]]
>>> list2
['name', ['price', 100]]
>>> list3
['name', ['price', 100]]
>>> list4
['name', ['price', 100]]
>>> list5
['name', ['price', 400]]	#深拷贝的对象与原对象不再相关
```

# hash值

```python
>>> hash({1,2,3})
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'set'
>>> hash(frozenset([1,2,3]))
-7699079583225461316
>>> hash(frozenset({1,2,3}))
-7699079583225461316
>>> hash(1.2)
461168601842738689
>>> hash(1.200)
461168601842738689
```

