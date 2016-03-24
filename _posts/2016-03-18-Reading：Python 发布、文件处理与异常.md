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

## 示例

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

## 常用命令

- 文件打开模式


| **打开模式** | **执行操作**                |
| -------- | ----------------------- |
| 'r'      | 以只读方式打开文件（默认）           |
| 'w'      | 以写入的方式打开文件，会覆盖已存在的文件    |
| 'x'      | 如果文件已经存在，使用此模式打开将引发异常   |
| 'a'      | 以写入模式打开，如果文件存在，则在末尾追加写入 |
| 'b'      | 以二进制模式打开文件              |
| 't'      | 以文本模式打开（默认）             |
| '+'      | 可读写模式（可添加到其他模式中使用）      |
| 'U'      | 通用换行符支持                 |

- 文件对象方法

| **文件对象方法**                     | **执行操作**                                 |
| ------------------------------ | ---------------------------------------- |
| f.close()                      | 关闭文件                                     |
| f.read([size=-1])              | 从文件读取size个字符，当未给定size或给定负值的时候，读取剩余的所有字符，然后作为字符串返回 |
| f.readline([size=-1])          | 从文件中读取并返回一行（包括行结束符），如果有size有定义则返回size个字符 |
| f.write(str)                   | 将字符串str写入文件                              |
| f.writelines(seq)              | 向文件写入字符串序列seq，seq应该是一个返回字符串的可迭代对象        |
| f.seek(offset, from)           | 在文件中移动文件指针，从from（0代表文件起始位置，1代表当前位置，2代表文件末尾）偏移offset个字节 |
| f.tell()                       | 返回当前在文件中的位置                              |
| f.truncate([size=file.tell()]) | 截取文件到size个字节，默认是截取到文件指针当前位置              |

- close()

文件在关闭之前一直是放在内存中的，关闭时才进行保存，因此要注意将文件关闭。

```python
>>> dialogue = open('C:\\dialogue.txt')
>>> dialogue
<_io.TextIOWrapper name='C:\\dialogue.txt' mode='r' encoding='cp936'>
```

##  os模块

- os模块中关于文件/目录常用的函数使用方法

| **函数名**           | **使用方法**                                 |
| ----------------- | ---------------------------------------- |
| getcwd()          | 返回当前工作目录                                 |
| chdir(path)       | 改变工作目录                                   |
| listdir(path='.') | 列举指定目录中的文件名（'.'表示当前目录，'..'表示上一级目录）       |
| mkdir(path)       | 创建单层目录，如该目录已存在抛出异常                       |
| makedirs(path)    | 递归创建多层目录，如该目录已存在抛出异常，注意：'E:\\a\\b'和'E:\\a\\c'并不会冲突 |
| remove(path)      | 删除文件                                     |
| rmdir(path)       | 删除单层目录，如该目录非空则抛出异常                       |
| removedirs(path)  | 递归删除目录，从子目录到父目录逐层尝试删除，遇到目录非空则抛出异常        |
| rename(old, new)  | 将文件old重命名为new                            |
| system(command)   | 运行系统的shell命令                             |
| walk(top)         | 遍历top路径以下所有的子目录，返回一个三元组：(路径, [包含目录], [包含文件]) |
|                   | *以下是支持路径操作中常用到的一些定义，支持所有平台*              |
| os.curdir         | 指代当前目录（'.'）                              |
| os.pardir         | 指代上一级目录（'..'）                            |
| os.sep            | 输出操作系统特定的路径分隔符（Win下为'\\'，Linux下为'/'）     |
| os.linesep        | 当前平台使用的行终止符（Win下为'\r\n'，Linux下为'\n'）     |
| os.name           | 指代当前使用的操作系统（包括：'posix',  'nt', 'mac', 'os2', 'ce', 'java'） |

- os.path模块中关于路径常用的函数使用方法

| **函数名**                     | **使用方法**                                 |
| --------------------------- | ---------------------------------------- |
| basename(path)              | 去掉目录路径，单独返回文件名                           |
| dirname(path)               | 去掉文件名，单独返回目录路径                           |
| join(path1[, path2[, ...]]) | 将path1, path2各部分组合成一个路径名                 |
| split(path)                 | 分割文件名与路径，返回(f_path, f_name)元组。如果完全使用目录，它也会将最后一个目录作为文件名分离，且不会判断文件或者目录是否存在 |
| splitext(path)              | 分离文件名与扩展名，返回(f_name, f_extension)元组      |
| getsize(file)               | 返回指定文件的尺寸，单位是字节                          |
| getatime(file)              | 返回指定文件最近的访问时间（浮点型秒数，可用time模块的gmtime()或localtime()函数换算） |
| getctime(file)              | 返回指定文件的创建时间（浮点型秒数，可用time模块的gmtime()或localtime()函数换算） |
| getmtime(file)              | 返回指定文件最新的修改时间（浮点型秒数，可用time模块的gmtime()或localtime()函数换算） |
|                             | *以下为函数返回 True 或 False*                   |
| exists(path)                | 判断指定路径（目录或文件）是否存在                        |
| isabs(path)                 | 判断指定路径是否为绝对路径                            |
| isdir(path)                 | 判断指定路径是否存在且是一个目录                         |
| isfile(path)                | 判断指定路径是否存在且是一个文件                         |
| islink(path)                | 判断指定路径是否存在且是一个符号链接                       |
| ismount(path)               | 判断指定路径是否存在且是一个挂载点                        |
| samefile(path1, paht2)      | 判断path1和path2两个路径是否指向同一个文件               |

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
{'__builtins__': <module '__builtin__' (built-in)>, 'each_line': 'hello world',
 '__doc__': None, 'data': <closed file 'dialogue.txt', mode 'r' at
 0x0000000003562D20>, '__name__': '__main__', '__package__': None, 'os': <module
 'os' from 'C:\Users\hamrf\Anaconda2\lib\os.pyc'>, 'sayhi': 'hello world'}
```

## pickle模块

```python
>>> import pickle
>>> list1 = [1,2,3,'chen',['hao','ran']]
>>> f = open('list1.pkl','wb')
>>> pickle.dump(list1,f)	#将数据以二进制的形式写入文件中
>>> f.close()

>>> f = open('list1.pkl','rb')
>>> mylist = pickle.load(f)	#从二进制的文件中读取原有数据
>>> mylist
[1, 2, 3, 'chen', ['hao', 'ran']]
>>> f.close()
```

# 异常处理

## 内置标准异常

| **错误类型**              | **错误类型说明**                               |
| --------------------- | ---------------------------------------- |
| AssertionError        | 断言语句（assert）失败                           |
| AttributeError        | 尝试访问未知的对象属性                              |
| EOFError              | 用户输入文件末尾标志EOF（Ctrl+d）                    |
| FloatingPointError    | 浮点计算错误                                   |
| GeneratorExit         | generator.close()方法被调用的时候                |
| ImportError           | 导入模块失败的时候                                |
| IndexError            | 索引超出序列的范围                                |
| KeyError              | 字典中查找一个不存在的关键字                           |
| KeyboardInterrupt     | 用户输入中断键（Ctrl+c）                          |
| MemoryError           | 内存溢出（可通过删除对象释放内存）                        |
| NameError             | 尝试访问一个不存在的变量                             |
| NotImplementedError   | 尚未实现的方法                                  |
| OSError               | 操作系统产生的异常（例如打开一个不存在的文件）                  |
| OverflowError         | 数值运算超出最大限制                               |
| ReferenceError        | 弱引用（weak reference）试图访问一个已经被垃圾回收机制回收了的对象 |
| RuntimeError          | 一般的运行时错误                                 |
| StopIteration         | 迭代器没有更多的值                                |
| SyntaxError           | Python的语法错误                              |
| IndentationError      | 缩进错误                                     |
| TabError              | Tab和空格混合使用                               |
| SystemError           | Python编译器系统错误                            |
| SystemExit            | Python编译器进程被关闭                           |
| TypeError             | 不同类型间的无效操作                               |
| UnboundLocalError     | 访问一个未初始化的本地变量（NameError的子类）              |
| UnicodeError          | Unicode相关的错误（ValueError的子类）              |
| UnicodeEncodeError    | Unicode编码时的错误（UnicodeError的子类）           |
| UnicodeDecodeError    | Unicode解码时的错误（UnicodeError的子类）           |
| UnicodeTranslateError | Unicode转换时的错误（UnicodeError的子类）           |
| ValueError            | 传入无效的参数                                  |
| ZeroDivisionError     | 除数为零                                     |

```python
>>> assert 1<0	#断言，通常使用在程序测试阶段
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AssertionError
>>> raise	
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
RuntimeError: No active exception to reraise
>>> 1/0
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ZeroDivisionError: division by zero
>>> raise ZeroDivisionError('除数为0的异常')	#指定某种类型的异常
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ZeroDivisionError: 除数为0的异常
```