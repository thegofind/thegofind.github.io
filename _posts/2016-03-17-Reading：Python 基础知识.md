---
layout: post
title:  "Reading：Python 基础知识"
date:   2016-03-17 09:00:13
categories: [Python, Reading]
permalink: /archivers/reading-python-fundament
---
# Python基础知识

##	Python简介

- Python是严格区分大小写的

### 变量

变量本身类型不固定的语言称之为动态语言，与之对应的是静态语言。

### 常量

所谓常量就是不能变的变量，比如常用的数学常数π就是一个常量。在Python中，通常用全部大写的变量名表示常量：

```python
PI = 3.14159265359
```

但事实上PI仍然是一个变量，Python根本没有任何机制保证PI不会被改变，所以，用全部大写的变量名表示常量只是一个习惯上的用法，如果你一定要改变变量PI的值，也没人能拦住你。

## 操作符

```python
>>> 10/3
3.3333333333333335
>>> 10//3   #整数的地板除//永远是整数，即使除不尽
3
```

## 字符与编码

### 转义

```python
>>> location = 'C:\now'
>>> location
'C:\now'
>>> print(location)
C:
ow
>>> location = 'C:\\now'    #使用\进行转义
>>> print(location)
C:\now
>>> location = r'C:\now\none\null'  #使用r进行批量转义
>>> location
'C:\\now\\none\\null'
>>> print(location)
C:\now\none\null
>>> location = r'C:\now\none\null\'  #使用r进行批量转义的字符串结尾不可为\
  File "<stdin>", line 1
    location = r'C:\now\none\null\'
                                  ^
SyntaxError: EOL while scanning string literal
>>> location = r'C:\now\none\null'+'\\'  #若结尾为\，使用批量转义的解决方案
>>> print(location)
C:\now\none\null\
```

## 编码

### ASCII、Unicode、UTF-8

- ASCII

最早的计算机在设计时采用8个比特（bit）作为一个字节（byte），所以，一个字节能表示的最大的整数就是255。

由于计算机是美国人发明的，因此，最早只有127个字母被编码到计算机里，也就是大小写英文字母、数字和一些符号，这个编码表被称为ASCII编码，比如大写字母A的编码是65，小写字母z的编码是122。

- Unicode

Unicode把所有语言都统一到一套编码里，这样就不会再有乱码问题了。
Unicode最常用的是用两个字节表示一个字符（如果要用到非常偏僻的字符，就需要4个字节）。

现代操作系统和大多数编程语言都直接支持Unicode。

- ASCII与Unicode的对比

字母A用ASCII编码是十进制的65，二进制的01000001；

汉字“中”已经超出了ASCII编码的范围，用Unicode编码是十进制的20013，二进制的01001110 00101101。

如果把ASCII编码的A用Unicode编码，只需要在前面补0就可以，因此，A的Unicode编码是00000000 01000001。

- UTF-8

如果你写的文本基本上全部是英文的话，用Unicode编码比ASCII编码需要多一倍的存储空间，在存储和传输上就十分不划算。

Unicode编码转化为“可变长编码”的UTF-8编码。UTF-8编码把一个Unicode字符根据不同的数字大小编码成1-6个字节，常用的英文字母被编码成1个字节，汉字通常是3个字节，只有很生僻的字符才会被编码成4-6个字节。如果你要传输的文本包含大量英文字符，用UTF-8编码就能节省空间。

- UTF-16

从技术上讲，UTF-16也是一种变长编码，但由于UTF-16不向后兼容ASCII，因此，实际使用它的程序很少。

### 计算机系统通用的字符编码工作方式

**在计算机内存中，统一使用Unicode编码，当需要保存到硬盘或者需要传输的时候，就转换为UTF-8编码。**

用记事本编辑的时候，从文件读取的UTF-8字符被转换为Unicode字符到内存里，编辑完成后，保存的时候再把Unicode转换为UTF-8保存到文件：

![](http://github.com/thegofind/thegofind.github.io/raw/master/img/00002.png)

浏览网页的时候，服务器会把动态生成的Unicode内容转换为UTF-8再传输到浏览器：

![](http://github.com/thegofind/thegofind.github.io/raw/master/img/00001.png)

所以你看到很多网页的源码上会有类似<meta charset="UTF-8" />的信息，表示该网页正是用的UTF-8编码。

## 字符串

由于Python的字符串类型是str，在内存中以Unicode表示，一个字符对应若干个字节。如果要在网络上传输，或者保存到磁盘上，就需要把str变为以字节为单位的bytes。

Python对bytes类型的数据用带b前缀的单引号或双引号表示：

```python
x = b'ABC'
```

要注意区分'ABC'和b'ABC'，前者是str，后者虽然内容显示得和前者一样，但bytes的每个字符都只占用一个字节。

以Unicode表示的str通过encode()方法可以编码为指定的bytes，例如：

```python
>>> 'ABC'.encode('ascii')
b'ABC'
>>> '中文'.encode('utf-8')
b'\xe4\xb8\xad\xe6\x96\x87'
>>> '中文'.encode('ascii')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
```

纯英文的str可以用ASCII编码为bytes，内容是一样的，含有中文的str可以用UTF-8编码为bytes。含有中文的str无法用ASCII编码，因为中文编码的范围超过了ASCII编码的范围，Python会报错。

在bytes中，无法显示为ASCII字符的字节，用\x##显示。

反过来，如果我们从网络或磁盘上读取了字节流，那么读到的数据就是bytes。要把bytes变为str，就需要用decode()方法：

```python
>>> b'ABC'.decode('ascii')
'ABC'
>>> b'\xe4\xb8\xad\xe6\x96\x87'.decode('utf-8')
'中文'
```

要计算str包含多少个字符，可以用len()函数：

```python
>>> len('ABC')
3
>>> len('中文')
2
```

len()函数计算的是str的字符数，如果换成bytes，len()函数就计算字节数：

```python
>>> len(b'ABC')
3
>>> len(b'\xe4\xb8\xad\xe6\x96\x87')
6
>>> len('中文'.encode('utf-8'))
6
```

如果.py文件本身使用UTF-8编码，并且也申明了# -*- coding: utf-8 -*-，打开命令提示符测试就可以正常显示中文。

### 输出格式化的字符串

%d	整数

%f	浮点数

%s	字符串

%x	十六进制整数

```python
>>> 'Hi, %s, you have $%d.' % ('Michael', 1000000)
'Hi, Michael, you have $1000000.'
>>> 'growth rate: %d %%' % 7    #使用%%可以转义%
'growth rate: 7 %'
```