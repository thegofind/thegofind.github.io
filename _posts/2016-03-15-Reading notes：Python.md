---
layout: post
title:  "Reading notes：Python"
date:   2016-03-15 09:00:13
categories: [Python, Reading notes]
permalink: /archivers/reading-notes-python
---
本文为读书笔记，书籍为《Head First Python》、《Beginning Python: From Novice to Professional》、《Core Python Programming》。

# 列表List

list是一个可变的有序表，其中可以存储任意类型的数据（包括混合类型），可称之为‘打了激素的数组’。

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


# 构建发布

## 准备文件

- nester.py

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

### 构建一个发布文件
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

### 将发布安装到Python本地副本中

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

- 实例1

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

- 实例2

```python
PS C:\whatever\python\nester> python setup.py sdist
PS C:\whatever\python\nester> python
>>> import nester   #正常导入未报错，因为此时是在模块所在目录下运行python，但局限性很大
```

- 本地副本的路径与卸载

副本安装路径为Lib\site-packages，如需卸载可直接删除。
包含了三个文件：nester.py；nester.pyc；nester-1.0.0-py2.7.egg-info。

### 发布文件目录解析

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

### 模块的导入和使用

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
### 在python shell中打开模块文件

```python
#导入 C:\whatever\python\nester\nester.py 文件
#sys.path 命令可以查看Python模块在计算机上的位置
#sys.path.append()和sys.path.pop(-1)对这些模块进行修改

import sys
sys.path.append("C:\\whatever\\python\\nester") 
import nester 
```

- 使用一个普通的import语句时，如import nester，这会指示Python解释器允许你使用命名空间限定访问nester函数。不过还可以更为特定，如使用from nester import print_list_item，会把指定的函数增加到当前命名空间中，此时需要注意是否存在同名冲突。


### 模块文件编码报错

在编写Python时，当使用中文输出或注释时运行脚本，会提示错误信息：
SyntaxError: Non-ASCII character '\xe5' in file *******

解决方法：在文件开头加入(一定要添加在源代码的第一行)

```python
# -*- coding: UTF-8 -*- 或者  #coding=utf-8
```