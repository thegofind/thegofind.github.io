---
layout: post
title:  "Reading：Python 发布、文件处理与异常"
date:   2016-03-18 09:00:13
categories: [Python, Reading]
permalink: /archivers/reading-python-publish-file-exception
---
本文为读书笔记，书籍为《Head First Python》、《Beginning Python: From Novice to Professional》、《Core Python Programming》。

# 构建发布

## 准备文件

- nester.py

模块是一个包含Python代码的文本文件

```python
# -*- coding: UTF-8 -*- 
def print_list_item(the_list):
	for each_item in the_list:
		if isinstance(each_item,list):
			print_list_item(each_item)
		else:
			print(each_item)
```

- setup.py

setup.py程序提供了模块的元数据，用来构建、安装和上传打包的发布。每次版本更新需要更改元数据version。

```python
from distutils.core import setup

setup(
	name = 'nester',
	version = '1.0.0',
	py_modules = ['nester'],
	author = 'thegofind',
	author_email = '43659894@qq.com',
	url = 'http://thegofind.github.io',
	description = 'a simple printer of nested lists',
)
```

## 构建一个发布文件
在nester.py（C:\whatever\python\nester\nester.py）所在目录下打开一个终端窗口，键入一行命令：python setup.py sdist

```python
PS C:\whatever\python\nester> python setup.py sdist
running sdist
running check
warning: sdist: manifest template 'MANIFEST.in' does not exist (using default file list)
warning: sdist: standard file not found: should have one of README, README.txt
writing manifest file 'MANIFEST'
creating nester-1.0.0
copying files to nester-1.0.0...
copying nester.py -> nester-1.0.0
copying setup.py -> nester-1.0.0
creating dist
creating 'dist\nester-1.0.0.zip' and adding 'nester-1.0.0' to it
adding 'nester-1.0.0\nester.py'
adding 'nester-1.0.0\PKG-INFO'
adding 'nester-1.0.0\setup.py'
removing 'nester-1.0.0' (and everything under it)
```

## 安装Python本地副本

在终端中，键入以下命令：python setup.py install

```python
PS C:\whatever\python\nester> python setup.py install
running install
running build
running build_py
creating build
creating build\lib
copying nester.py -> build\lib
running install_lib
running install_egg_info
Removing C:\Users\hamrf\Anaconda2\Lib\site-packages\nester-1.0.0-py2.7.egg-info
Writing C:\Users\hamrf\Anaconda2\Lib\site-packages\nester-1.0.0-py2.7.egg-info
```

- 把模块安装到我的Pyhon本地副本是否有必要？

Python会在特定的一组位置寻找模块（详见import sys;sys.path），如果模块未在Python的路径列表中，就会导致ImportError错误。使用发布工具来构建模块并安装到你的Python本地副本中，就能避免这种错误。

- 示例1

```python
PS C:\whatever\python\nester> python setup.py sdist
PS C:\> python
>>> import nester
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: No module named nester   #在路径列表中未发现该模块
PS C:\whatever\python\nester> python setup.py install
PS C:\> python
>>> import nester   #正常导入未报错
```

- 示例2

```python
PS C:\whatever\python\nester> python setup.py sdist
PS C:\whatever\python\nester> python
>>> import nester   #正常导入未报错，因为此时是在模块所在目录下运行python，但它很‘脆弱’
```

- 本地副本的路径与卸载

副本安装路径为Lib\site-packages，如需卸载可直接删除。
包含了三个文件：nester.py；nester.pyc；nester-1.0.0-py2.7.egg-info。

## 发布文件目录解析

```python
#安装前
-nester
    -nester.py  #代码在这个文件中
    -setup.py   #元数据在这个文件中

#安装后
-nester
    -MANIFEST   #这个文件中包含发布中的文件列表
    -build
        -lib 
            -nester.py
    -dist
        -nester-1.0.0.zip   #发布包
    -nester.py
    -nester.pyc   #‘编译’版本的代码
    -setup.py
```

## 模块的导入和使用

```python
>>> movies = ['the holy grail',1975,['graham chapman',['maichael palin','john cleese','terry jones']]]
>>> import nester
>>> nester.print_list_item(movies)  #如果不指明模块，会报错
the holy grail
1975
graham chapman
maichael palin
john cleese
terry jones
```

## 在python shell中载入模块文件

- 使用一个普通的import语句时，如import nester，这会指示Python解释器允许你使用命名空间限定访问nester函数。不过还可以更为特定，如使用from nester import print_list_item，会把指定的函数增加到当前命名空间中，此时需要注意是否存在**同名冲突**。

```python
#导入 C:\whatever\python\nester\nester.py 文件
#sys.path 命令可以查看Python模块在计算机上的位置
#sys.path.append()和sys.path.pop(-1)对这些模块进行修改

import sys
sys.path.append("C:\\whatever\\python\\nester") 
import nester 
>>> list = ['a','b',['c','d','e',['f','g'],'h']]
>>> nester.print_list_item(list)	#因import nester时未指定函数，因此此处前缀nester为必须
a
b
c
d
e
f
g
h
```

# 模块文件

## 模块的加载

绝大部分的模块创建的目的是为了被别人调用而不是作为独立执行的脚本。

最外层代码（全局代码，没有缩进的代码行）——在模块没有被导入时就会被执行，不管是否真的需要执行。因此，比较安全的写代码方式就是除了真正需要执行的代码外，几乎所有的功能代码都在函数中。通常只主体程序模块中有大量的顶级可执行代码。

```python
#如果模块/类是被导入，__name__的值为模块/类名称
#如果模块是被直接执行，__name__的值为'__main__'

if __name__ == "__main__":
    代码块	#当此模块为主体程序时，才执行此处代码。
```

## 模块的测试

测试代码仅当该文件被直接执行时运行，因此可以利用\_\_name\_\_这个变量，将测试代码放入一个函数中，如果该是被当成脚本运行，则调用这个函数。

每次代码更新都运行这些测试代码，以确认修改没有引发新问题。

## 模块文件编码问题

在编写Python时，当使用中文输出或注释时运行脚本，会提示错误信息：
SyntaxError: Non-ASCII character '\xe5' in file *******

解决方法：在文件开头加入(一定要添加在源代码的第一行)

```python
# -*- coding: UTF-8 -*- 或者  #coding=utf-8
```

# 文件处理

## 文件操作示例

- 待读取文件：dialogue.txt

```python
Tom: Hi, Jack, I will hold a party at my house next Monday. Would you like to come?
Jack: Of course! I'd like to. 
Tom: Great! I am happy to hear that. You can take a friend with you. 
Jack: OK. Well, what's the theme of the party?
Tom: It's a dancing party. So you can show us your dancing skills that night.
Jack: Wow, sounds interesting!
Tom: I'm glad you like it. So let's meet each other next Monday. 
Jack: Hold on! Jim , you haven't told me when it starts yet.  
Tom: Oh, I'm sorry.The party will start at 8：00.
Jack: Well. See you then.
```

- 读取脚本：readfile.py

```python
# -*- coding: UTF-8 -*-
from __future__ import print_function	#python3之前的版本无法使用print('hello',end='')
import os
try:
	data = open('dialogue.txt')
	for each_line in data: 
		try:
			(role,line_spoken)=each_line.split(':',1)	#1、多重赋值；2、split()第二个参数为可选参数，默认是寻找最多的可能性
			print(role,end=':\n')
			print(line_spoken,end='')
		except ValueError:	#数据不符合期望的格式会出现ValueError
			pass	#pass语句是Python的空语句或null语句，它什么也不做
	    finally：
			data.close()
except IOError:	#数据无法正常访问时会出现IOError
	print('the data file is missing!')
```

- 执行结果

```python
PS C:\whatever\python\test> python readfile.py
Tom:
 Hi, Jack, I will hold a party at my house next Monday. Would you like to come?
Jack:
 Of course! I'd like to.
Tom:
 Great! I am happy to hear that. You can take a friend with you.
Jack:
 OK. Well, what's the theme of the party?
Tom:
 It's a dancing party. So you can show us your dancing skills that night.
Jack:
 Wow, sounds interesting!
Tom:
 I'm glad you like it. So let's meet each other next Monday.
Jack:
 Hold on! Jim , you haven't told me when it starts yet.
Tom:
 Oh, I'm sorry.The party will start at 8：00.
Jack:
 Well. See you then.
```

## 文件操作常用命令

os.getcwd()     #获取当前目录

os.chdir()   #修改当前目录

os.path

data = open(filename) 

data.readline()

data.seek()     #跳至文件的第n个位置，常用seek()跳至起始位置

data = open(filename,w+)

print('hello world',file=data)  #将hello world写入文件中

data.close

- 示例代码

```python
>>> import os
>>> os.getcwd()  
'C:\\Users\\hamrf'  
>>> os.chdir('c:/')  
>>> os.getcwd()
'c:\\'
>>> data = open('dialogue.txt')
>>> print data.readline()
Tom: Hi, Jack, I will hold a party at my house next Monday. Would you like to come?
>>> data.seek(2)    #seek()第一个参数为偏移量，第二个参数为起始位置，默认为文章开头
>>> print data.readline()
m: Hi, Jack, I will hold a party at my house next Monday. Would you like to come?
>>> data.close()
```

- locals

data = open(filename) 
if 'data' in locals() #locals()会返回当前作用域中定义的所有名的一个集合

```python
>>> locals()
{'__builtins__': <module '__builtin__' (built-in)>, 'each_line': 'hello world', '__doc__': None, 'data': <closed file 'dialogue.txt', mode 'r' at 0x0000000003562D20>, '__name__': '__main__', '__package__': None, 'os': <module 'os' from 'C:\Users\hamrf\Anaconda2\lib\os.pyc'>, 'sayhi': 'hello world'}
```

- close()

文件在关闭之前一直是放在内存中的，关闭时才进行保存，因此要注意将文件关闭。

```python
>>> dialogue = open('C:\\whatever\\python\\test\\dialogue.txt')
>>> dialogue
<_io.TextIOWrapper name='C:\\whatever\\python\\test\\dialogue.txt' mode='r' encoding='cp936'>
```

# 异常处理