---
title: git疑难杂症总结
date: 2016-09-21 09:52:33
categories: Git
tags: [git疑难杂症]
---
## git远程仓库同步,历史提交不同导致错误
### 问题描述：
![问题](http://upload-images.jianshu.io/upload_images/1654122-69115bc264b92f2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
<!-- more -->
这是在2.9版本git之后出现的问题，就是当发现不同的历史时（已经有人提交过，一般情况下只会出现一次这样的情况）
### 解决办法:
```
git pull origin master --allow-unrelated-histories
```

## .gitignore不生效
### 问题描述：
之前add进版本库的文件，现在添加了一个.gitignore文件后，不希望之前跟踪过的文件被跟踪了

### 解决办法：
```
git rm -r --cached . #删除追踪状态
git add . 
git commit -m "fixed untracked files"
```
## .git文件夹导致不能跟踪文件
### 问题描述：本地git add .后查看git  status 
出现modified: xxx(modified content, untracked content)
大概意思是xxx目录没有被跟踪。那自然push上去的时候是空的了
解决办法：
主要是xxx目录下有一个.git 目录，删除.git目录，重新git add .后续操作
