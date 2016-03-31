---
layout: post
title:  "读书笔记：Python 迭代器、生成器、range()"
date:   2016-03-22 09:00:13
categories: [Python, 读书笔记]
permalink: /archivers/reading-python-range
---
本文为读书笔记，书籍为《Head First Python》、《Python基础教程》、《Python核心编程》。

# 条件和循环

## if、else、ifelse

## break、continue

- 当遇到continue语句时，程序会终止当前循环，并忽略剩余的语句，然后回到循环的顶端。在开始下次迭代前，验证循环条件，验证成功才会开始下一次迭代。


## while-else、for-else

- 在循环中使用else时，else子句只在循环完成后执行，也就是说break语句也会跳过else块。

## for

for循环会访问一个可迭代对象（例如序列或是迭代器）中的所有元素，并在所有条目都处理过后结束循环。

# 内建函数

## sorted() 返回列表

sorted(iterable,key=None, reverse=False])

```python
>>> dict1 = {'a':(1,6),'b':(2,5),'A':(3,4)}
>>> sorted(dict1)	#等同于sorted(dict1.keys())
['A', 'a', 'b']
>>> sorted(dict1.values(),key = lambda x : x[0])	#这里匿名函数的作用是将每一个value作为一个序列进行索引取值
[(1, 6), (2, 5), (3, 4)]
>>> sorted(dict1.values(),key = lambda x : x[1])
[(3, 4), (2, 5), (1, 6)]
```

## zip() 返回列表

## reversed()	返回迭代器

## enumerate() 返回迭代器

```python
>>> list1 = ['a','b','c']
>>> dict(enumerate(list1))
{0: 'a', 1: 'b', 2: 'c'}
```


## range()

class range(stop)
class range(start, stop[, step])

若stop-start为正，即递增时，step必须同为正，否则序列为空。

若stop-start为负，即递减时，step必须同为负，否则序列为空。

- range的 优势

> The advantage of the [`range`](https://docs.python.org/3/library/stdtypes.html#range) type over a regular [`list`](https://docs.python.org/3/library/stdtypes.html#list) or [`tuple`](https://docs.python.org/3/library/stdtypes.html#tuple) is that a [`range`](https://docs.python.org/3/library/stdtypes.html#range) object will always take the same (small) amount of memory, no matter the size of the range it represents (as it only stores the `start`, `stop` and `step` values, calculating individual items and subranges as needed).


> 同list或tuple相比，range类型有一个优势：无论range包含的数据有多少，range总是只消耗很小、恒定的内存。（因为它只保存起始位置、终止位置和步数，根据需要单独计算每个元素。


- 支持包容测试、 索引查找、 切片、迭代

```python
>>> r = range(1,10,2)
>>> 1 in range(1,10,2)	#包容测试
True
>>> r.index(3)	#元素索引查找
1
>>> r[0:2]	#切片
range(1, 5, 2)
>>> list(r[0:2])
[1, 3]
>>> r[-1]
9
```

- == 和 !=

range()可以被当作序列，使用 == 和 != 进行比较，如果它们表示相同的值序列，则被视为相等。

```#python
>>> range(0) == range(2,1,2)
True
>>> range(1,5,3) == range(1,6,3)
True
```

# 迭代器

迭代器对象有一个\_\_next\_\_()方法，而不是通过索引来计数，调用后返回下一个条目，所有条目迭代完成后，迭代器引发一个StopIteration异常告诉程序循环结束。for语句在内部调用_\_next\_\_()并捕获异常。

迭代器的限制：无法向后移动，不能回到开始，每一个迭代器之间都是相互独立的。

## 迭代器的特性

- 文件对象生成的迭代器会自动调用readline()方法，这样就可以使用简单的for eachLine in f 替换 for eachLine in f.readline()。


- 在迭代可变对象的时候修改它们并不是一个好主意，使用字典keys()方法时是可以的，因为keys()返回一个独立于字典的列表。

```python
for eachURL in allURLs:
    if not eachURL.startswith('http://'):
        allURLs.remove(eachURL)
```

## 如何创建迭代器？

# 相关内容

## 三元表达式

```python
>>> x=1
>>> y=2
>>> num = x if x<y else y
>>> num
1
```

## 列表解析

列表解析的表达式可以取代内建的map()函数以及lambda，而且效率更高。

```python
>>> list1 = ['hello world','chen haoran']
>>> list([ len(word) for eachItem in list1 for word in eachItem.split(' ')])
[5, 5, 4, 6]
```

## 生成器表达式

列表解析的一个不足就是必要生成所有的数据，用以创建整个列表，这可能对有大量数据的迭代器有负面效应，生成器表达式是列表解析的一个扩展，通过结合列表解析和生成器解决了这个问题。

生成器使用了“延迟计算”，并不真正创建列表，而是返回一个生成器，这个生成器在每次计算出一个条目后，把这个条目“产生”出来。

```python
# 列表解析
[expr for iter_var in iterable if cond_expr]
# 生成器表达式
(expr for iter_var in iterable if cond_expr)
```