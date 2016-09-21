---
title: git教程(一)
date: 2016-09-21 09:49:33
categories: Git
tags: [git安装初始化配置]
---
## git安装和初始化配置
什么是git，为什么用git，这些百度一大堆，我就不废话了，直接进入怎么用这个环节。
git有远程仓库和本地仓库之分，远程仓库必须是裸仓库，本地仓库之间可以互相clone但是不能进行pull，push等提交和拉取远程数据，只有裸仓库才能进行多用户针对远程仓库的数据交互操作。这里的仓库类型后面会有介绍，不懂得看到后边就懂了。
<!-- more -->
### git安装
我用的是windows系统，这里只粘贴一下windows的git地址
```
     https://git-for-windows.github.io/#contribute   window的git安装网址
```
安装属于傻瓜式，下一步、下一步、下一步、、、完成。
安装完成之后你可以用鼠标右键一下，有git base here(输入shell命令，ssh命令等，最重要的是git命令可以敲了，其他的别管了)和git gui here(小白的图形化界面，不过我强烈不推荐用这个，后面的教程全部都是命令行形式的，没几个命令，也属于装逼神器)这两个选项了代表成功了。
### git配置
对于团队开发，用版本控制工具很大程度上是为了团队之间开发的沟通，git配置也就是设置一些自己在这个项目中的身份和标识，这样以后看开发日志就可以解决一些代码冲突等的一些问题。下面的带--global的代表的是全局的，你可以在~/.gitconfig文件中看到你的所有配置，你可以直接编辑它。
```
命令：git config --global user.name XXX设置姓名
          git config --global user.email xxx@xxx.com设置邮箱
          git config --get user.name查看姓名
          git config --global user.name查看姓名
          git config --global --add user.name XX增加姓名，不过以最后一个add的为主
          git config --global --unset user.name XX去除姓名
          git config --list --global查看global配置
          git config --global alias.xx "比如log oneline"设置快捷命令
比如git log --oneline --graph配置为git config --global alias.mylog log --oneline --graph 以后你就可以这样敲了git mylog（我是没用过，还是喜欢敲满字符，也懒得设置了）

```
![配置](http://upload-images.jianshu.io/upload_images/1654122-f4121a45e9a244f0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![查看配置](http://upload-images.jianshu.io/upload_images/1654122-33cf46a0c62a2e3e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![修改配置](http://upload-images.jianshu.io/upload_images/1654122-867ae6fa56582b49.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
###git Rsa配置
当你与服务器交互时，一定会设置代码安全方面的问题，这个就好比是你登陆系统时，服务器会要求你输入用户名和密码一样，git与服务器之间也是这样，必须要标识你是可以操作的用户，这样才能保证你是被赋予管理代码权限的用户。但是输入用户名密码操作太过于繁琐，所以git不止支持http协议，更支持ssh协议，ssh协议地址，你可以用私钥公钥的形式让服务器验证你的权限。

![生成公钥私钥](http://upload-images.jianshu.io/upload_images/1654122-bbf3a46e2ac53559.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![github中ssh public key（公钥）的添加](http://upload-images.jianshu.io/upload_images/1654122-86a788d3c7961f30.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
ok，现在你可以用ssh命令连接一下远程仓库了。

![公钥私钥的验证登陆](http://upload-images.jianshu.io/upload_images/1654122-a276aa422309c0e7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)