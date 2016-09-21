---
title: Python教程(一)
date: 2016-9-19
categories: other
tags: [Python入门语法]
---
## Python的特点
Python是一门解释性语言，它是多种语言的精华的集合，它从Perl（另外一种远远超越了标准的shell脚本的脚本语言），python的正则表达式引擎就是基于Perl（它最大的优势就在于它的字符串模式匹配能力）。Python和java比较来说，他们都有类似的面向对象特性和语法，他们之间还诞生了一个可以在只有java环境中于运行Python程序的只用java编写的Python解释器Jython。JavaScript也是类似Python的面向对象脚本语言，但是JavaScript是基于原型设计的，Python是遵循面向对象系统，二者的类和对象有一些 差异。其他的就不记录了，太多了，记录主要的几个自己知道的语言。
1. 一般的Python解释器是用C编写的，又叫CPython
2. 相对的用java编写的就是前面提到的JPython，不作描述。
3. IronPython，用C#语言完成，适用环境为.NET和Mono
4. Stackless是针对CPython的修改。
<!-- more -->
## Python起步
### 程序输出

```
>>> 'hello'#交互式解释器调用repr()函数来显示对象
'hello'
>>> print('hello')#print语句调用str()函数显示对象
hello
```
```
>>> print('姓名：%s 年龄：%d'%('python',23))  # 输出格式化，占位符
姓名：python 年龄：23
```
```
>>> user = input()  # 控制台输入
>? dlfkjs
>>> print(user)
dlfkjs
```
```
>>> 3+2
5
>>> 3-2
1
>>> 3 * 2
6
>>> 3 / 2
1.5
>>> 3 // 2  # 整除
1
>>> 3 ** 2  # 3的2次方
9
>>> 3 % 2  # 模
1
```
```
## 标准比较运算符
>>> 1 < 2
True
>>> 1 <= 2
True
>>> 1 > 2
False
>>> 1 >= 2
False
>>> 1 ==2
False
>>> 1 != 2
True
```
```
## 逻辑运算符
>>> 4>2 and 2>4  # 变种>>> 2 < 4 < 2
False
>>> 4>2 or 2>4
True
>>> not 2 > 4
True
```
```
>>> num = 4
>>> num *= 10  # 相当于num = num * 10
>>> print(num)
40
# 不过python不支持num++ 和num--等，++num就是num，--num也相当于num
```
```
>>> 'asdf'[0]
'a'
>>> 'asdf'*3
'asdfasdfasdf'
>>> 'asdf'[1 : 3]
'sd'
>>> 'asdf'[1:]
'sdf'
>>> 'asdf'[:3]
'asd'
>>> 'asdf'[:]
'asdf'
>>> 'asdf'[-1:]
'f'
>>> 'asdf'[:-3]
'a'
```
```
# 列表是有序的，元组是有序的
>>> aList = [1, 'a', 2, 'b', 3, 'c']  # 列表切片还是列表
>>> aList[1:5]
['a', 2, 'b', 3]
>>> aTuple = (1, 'a', 2, 'b', 3, 'c')  # 元组切片还是元组
>>> aTuple[1:5]
('a', 2, 'b', 3)
```
```
# 元素是无序的
>>> aDict = {'aaa':'bbb'}
>>> aDict['111'] = 333
>>> aDict
{'111': 333, 'aaa': 'bbb'}
>>> aDict['222'] = 444
>>> aDict
{'111': 333, '222': 444, 'aaa': 'bbb'}
>>> aDict.keys()
dict_keys(['111', '222', 'aaa'])
>>> aDict['111']
333
>>> for key in aDict:
...     print('key = ',key,'valse = ',aDict[key])
...     
key =  111 valse =  333
key =  222 valse =  444
key =  aaa valse =  bbb
```
```
# if语句
>>> if 1 > 2:
...     print('1')
... elif 2 > 3:
...     print('2')
... else:
...     print('3')
...     
3
# while语句
>>> while x < 3:
...     print(x)
...     x += 1
...     
0
1
2
# for循环和range()内建函数
>>> foo = 'abcde'  # 遍历字符串中的字符
>>> for i in range(len(foo)):
...     print(foo[i],'%d'%i)
...     
a 0
b 1
c 2
d 3
遍历key，char的简易用法
>>> for i ,ch in enumerate(foo):
...     print(ch,i)
...     
a 0
b 1
c 2
d 3
e 4
# 列表解析
>>> squared = [x ** 2 for x in range(4)]
>>> for i in squared:
...     print(i)
...     
0
1
4
9
# 后边还可以进行筛选循环之后的结果后，进行最后解析
>>> squared = [x ** 2 for x in range(4) if not x % 2]  # 0是False，1是True
>>> for i in squared:
...     print(i)
...     
0
4
```
## 异常的捕捉
```
import sys
try:
    raiseexcept Exception as e:
    t,v,tb = sys.exc_info()  # 异常的类型名称，异常实例，追溯异常流程对象traceback
    print(t,v,tb)
```
## 文件和内建函数open()和file()
```
fileName = '.gitignore'
file = open(fileName, 'r')
try:
    for line in file:  # 最快的读取每一行文本的方法        
        # lineline = line.rstrip('\n') # 去除每行末尾的\n符号
        lineline = line.rstrip()  # 去除\n和空白符
        print(lineline)
    file.close()
except Exception as e:  # python3要求我们的异常必须继承Exception类，这样可以捕获所有异常
           print(e)
print('-'*20)
file = open(fileName)
try:
    splitlines = file.readlines()  # 将所有行一次读出来    
    # splitlines = file.read().splitlines()  # 将\n给去除
    splitlines = file.read().split('\n')  # 另一种方法去除
    file.close()
    for line in splitlines:
        print(line,end='')
except Exception as e:  # python3要求我们的异常必须继承Exception类，这样可以捕获所有异常
             print(e)
```