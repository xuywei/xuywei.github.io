---
title: git教程(三)
date: 2016-09-21 09:52:33
categories: Git
tags: [git同步远程仓库]
---
## git的远程和本地的同步交互操作
>当本地的文件被git管理后，我们之前讲过了一些本地的操作，主要都是在工作区和暂存区进行操作，但是我们不能只在本地玩啊是不是，这不，我们下面就讲同步你的本地代码到服务器上去，也叫远程仓库。

<!-- more -->
## 再贴一下git的本地操作
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1654122-d7fe4a18bdef896d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 下面要在服务器里创建一个仓库，我们这里没有自己的，就用第三方的吧，这里自己百度一下好吧~~~~~~~~

```
git remote add 远程仓库的名字 远程地址  # 对应的中文对应你的仓库名字和地址，地址是你创建仓库之后给你的唯一地址，名字的话，默认是origin，当然你你可以自己起名，比如baby哈哈
git pull origin master
git push origin master
git push -u origin master#以后提交都是origin master，以后就只打git push就行了，pull同理
git pull -u origin master
```
ok了，去远程仓库看一下，代码都同步好了。