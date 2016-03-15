---
layout: post
title:  "Reading notes：Python"
date:   2014-12-30 09:00:13
categories: [Python, Reading notes]
permalink: /archivers/reading-notes-python
---
本文为读书笔记，书籍为《Head First Python》、《Beginning Python: From Novice to Professional》、《Core Python Programming》。

## 列表List

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

- 在python shell中打开模块文件

```python
#导入 C:\whatever\python\nester\nester.py 文件
#sys.path 命令可以查看Python模块在计算机上的位置
#sys.path.append()和sys.path.pop(-1)对这些模块进行修改

import sys
sys.path.append("C:\\whatever\\python\\nester") 
import nester 

```

- 模块文件编码报错：

在编写Python时，当使用中文输出或注释时运行脚本，会提示错误信息：
SyntaxError: Non-ASCII character '\xe5' in file *******

解决方法：
在文件开头加入(一定要添加在源代码的第一行)：

```python
# -*- coding: UTF-8 -*- 或者  #coding=utf-8
```