---
title: pip的更新及常用第三方模块的安装
date: YYYY-MM-DD HH:MM:SS
categories: 
tags: [python,pip]
---
## pip更新
```
python2 -m pip install --upgrade pip
python3 -m pip install --upgrade pip #这里的python依据你的python名字，我个人改为了python2和python3，是为了兼容2个版本
```
<!-- more -->

## win10中python2和python3并存，安装模块
```
python2 -m pip install xxx
python3 -m pip install xxx
```
## windows上python无法安装lxml
### 下载模块
[lxml](http://www.lfd.uci.edu/~gohlke/pythonlibs/#lxml)，从这里找到对应的whl文件下载
### 安装wheel
pip3 install wheel
### 安装下载的模块
安装lxml
pip3 install 下载的对应模块路径