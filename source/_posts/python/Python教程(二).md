---
title: Python教程(二)
date: 2016-9-19
categories: other
tags: [Python函数]
---
## 函数的5类参数
>python函数可以传入5类参数，顺序为：必选参数-默认参数-可变参数/命名关键字参数-关键字参数。其中可变参数和命名关键字参数不能组合使用。

<!-- more -->
## Python的函数
```
class TestClass(object):
    version = 1.9
    def __init__(self,nm='xuwei'):
        self.name = nm
        print(nm)
    def showName(self):
        print(self.name)
    def showVersion(self):
        print(self.version)
myClass=TestClass()
myClass.showName()
myClass.showVersion()
```
```
import sys
sys.stdout.write('Hello World!\n')  # 代码的输出，调用了标准的write()方法
print(sys.platform)  # 平台
print(sys.version)  # python版本
# help(dir)
print(int('5'))  # 5
print(len('sdfas'))  # 5
print(list(range(5, 15, 2)))  # [5, 7, 9, 11, 13]
print(str(3434))  # 3434
print(type(56))  # <class 'int'>
# input('输入：')  # 输入：45
print(dir(sys))  # sys模块中的所有属性
print(dir)  # <built-in function dir>
print(dir())  # 没有提供参数，就是显示全局变量的名字的列表print(type(dir))  # <class 'builtin_function_or_method'>
```