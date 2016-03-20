---
layout: post
title:  "Reading：Python 对象、内建函数"
date:   2016-03-19 09:00:13
categories: [Python, Reading]
permalink: /archivers/reading-python-fundame
---

# 内建函数BIF

built-in是\_\_builtins\_\_模块的成员，在你的程序开始或在交互解释器中给出>>>提示之前，由解释器自动导入，把它们看成适用在任何一级Python代码的全局变量。

函数内部的模块导入代码不会被执行，除非函数正在执行。

## print

```python
>>> print(5+3)
8
>>> 5+3
8
>>> print('hello '+'world')
hello world
>>> print('300+200=',300+200)   #逗号隔开的内容会被拼接起来，并间隔一个空格
300+200= 500
>>> print('hello world ' * 2)
hello world hello world
>>> print('hello world\n' + 2)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: cannot concatenate 'str' and 'int' objects
```

## dir(______builtins______)

```python
# BIF：built-in functions 内置函数有自己的命名空间，名为__builtins__
# dir(__builtins__) 查看Python内置函数BIF 
# help(input) 查看函数说明

>>> dir(__builtins__)	#为了节省空间，以下BIF经过大量删减
['ArithmeticError', 'AssertionError','_', '__build_class__', '__debug__', '__doc__', '__import__', '__loader__', '__name__', '__package__', '__spec__', 'abs','set', 'setattr', 'slice', 'sorted', 'staticmethod', 'str', 'sum', 'super', 'tuple', 'type', 'vars', 'zip']
>>> import nester
>>> dir(nester)	   #查看模块nester的内置函数
['__doc__', '__loader__', '__name__', '__package__', '__path__', '__spec__']
>>> nester.__path__
_NamespacePath(['C:\\whatever\\python\\nester'])
>>> dir(str)	#查看字符串的内置函数，为了节省空间，以下内容经过大量删减
['__add__', '__class__', '__contains__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getnewargs__', 'split', 'splitlines', 'startswith', 'strip', 'swapcase', 'title', 'translate', 'upper', 'zfill']
```

## help()

```python
>>> sayhi = 'hello world'
>>> help(sayhi.split)	#如需查询字符串的某个内置函数，可以先创建一个字符串
Help on built-in function split:

split(...)
    S.split([sep [,maxsplit]]) -> list of strings

    Return a list of the words in the string S, using sep as the
    delimiter string.  If maxsplit is given, at most maxsplit
    splits are done. If sep is not specified or is None, any
    whitespace string is a separator and empty strings are removed
    from the result.
```

## module.\_\_doc\_\_

```python
>>> import sys
>>> print(sys.__doc__)	#查看sys模块的模块文档（功能和变量含义）
This module provides access to some objects used or maintained by the
interpreter and to functions that interact strongly with the interpreter.

Dynamic objects:	#为节省空间，此文档经大量删减

argv -- command line arguments; argv[0] is the script pathname if known
path -- module search path; path[0] is the script directory, else ''
modules -- dictionary of loaded modules
```

# 对象

## 与对象相关的内建函数

- id() 任何对象的身份可以使用内建函数id()来得到


- type() 通过内建函数type()可以查看Python对象的类型，type()返回的是对象而不是字符串。type是Python所有类型的根和所有Python标准类的默认元类。


- is 和 is not操作符可以测试两个变量是否指向同一个对象

```python
>>> a = '123'
>>> b = '123'
>>> a is b
True
>>> id(a) == id(b)	#同a is b等同
True
```

- 对象类型判断 isinstance()

```python
>>> a = 'chen'
>>> isinstance(a,(str,int))	#可以接受一个元组作为参数
True
```

## 创建对象

```python
#整型对象和字符串对象是不可变对象，所以Python会很高效地缓存它们。而浮点数或其他数据类型则不是。
>>> num1 =4.3
>>> num2 =3.3+1	#浮点数对象的创建
>>> id(num1)
2288929186080
>>> id(num2)
2288929186152
>>> num3 = 4
>>> num4 = 3+1	#整型对象的创建
>>> id(num3)
1739386768
>>> id(num4)
1739386768
>>> str1 = 'helloworld'
>>> str2 = 'hello'+'world'	#字符串对象的创建
>>> id(str1)
2288934543152
>>> id(str2)
2288934543152
```

##对象回收机制：引用计数

当一个对象的最后一个引用被删除，会导致该对象从此“无法访问”。从此刻起，该对象就称为垃圾回收机制的回收对象。

- 增加引用计数

对象被创建	x=3.14

别名被创建	y=x

参数传递		foorbar(x)

称为容器对象的一个元素	list=[123,x,'456']

- 减少引用计数

函数结束 foorbar(x)

对象被显式销毁	del y

对象的一个别名被赋值给其他对象 x='123',x=123

对象被容器移除 list.remove(x)

# 数据类型

## 整型

Python2的长整型类型能表达的数值仅仅与你的机器支持的（虚拟）内存大小有关。Python3中只有一种整数类型int。

## 双精度浮点型

```python
>>> num = 436.5
>>> isinstance(num,float)
True
```

## 复数

复数的表示为x+yj，其中x为实数部分，y为虚数部分。

- 实数部分和虚数部分都必须是浮点型
- 虚数部分必须有后缀j

示例：64.375-8.5j

## 类型转换

Python解决数字类型强制转换问题时，会强制将一个操作数转换为同另一个操作数相同的数据，优先顺序是：复数、浮点型。

工厂函数就是指这些内建函数都是类对象，当你调用它们时，实际上是创建了一个类实例。

```pytho
>>> bool(0)
False
>>> bool(None)
False
>>> bool(-1)	#复数的布尔值为True
True
>>> float('22.2')
22.2
>>> int('1101')		
1101
>>> int('1101',2)	#第二个参数默认是10
13
```

## 算数操作符号

```pytho
>>> -1/2    
-0.5        
>>> -1//2   #操作符//又称地板除，取小于或等于商的最大整数
-1                 
>>> -5//2   
-3          
>>> 1//2    
0 
>>> -3**2
-9
>>> (-3)**2	 	#括号的合理使用可以降低错误率，提高可读性
9
```

## 功能函数

```python
>>> abs(-1.3)	#取绝对值
1.3
>>> divmod(10,3)	#返回包含商与余数的元组
(3, 1)
>>> pow(2,4)	#指数运算
16
>>> round(1.4999)	#round四舍五入，第二个参数为小数点后精确位数
1
>>> round(1.4999,1)
1.5
>>> round(1.4499,1)
1.4
>>> round(1.4499,2)
1.45
```



# 性能优化

- 字符串的连接，使用join代替+

```python
>>> sayhi = 'hello '+'world '+'!'	#Python必须为每一个参加连接操作的字符串分配新的内存，包括新产生的字符串
>>> sayhi
'hello world !'
>>> sayhi = ''.join(['hello ','world ','!'])	#将子字符串放到一个可迭代的对象中(列表、元组等)
>>> sayhi
'hello world !'
```

- 列表的合并，使用extend()代替+

```python
>>> list1 = [1,2,3]
>>> list2 = [4,5,6]
>>> print(list1+list2)
[1, 2, 3, 4, 5, 6]
>>> list1.extend(list2)		#类似地，列表的合并推荐使用extend()
>>> print(list1)
[1, 2, 3, 4, 5, 6]
```

- 循环的性能优化

```python
#优化前
while i < len(myString)

#优化后
length = len(myString)
while i < length

#优化前
if each_item in alphas + nums:

#优化后
alphnums = alphas + nums
if each_item in alphnums:
```









类似os.linesep这样的名字需要解释器做两次查询（1）查找os以确认是一个模块，（2）在这个模块中查找linesep变量。如果在一个函数中频繁使用一个属性，建议为该属性取一个本地变量别名。

```python
>>> str1 = 'hello'
>>> str2 = 'world'
>>> str3 = str1 +str2
>>> str4 = 'hello' + 'world'
>>> id(str3)
2160033895024
>>> id(str4)
2160033897648
>>> id('hello')
2160034387536
>>> id(str1)
2160034387536
```





