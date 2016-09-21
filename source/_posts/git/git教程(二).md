---
title: git教程(二)
date: 2016-09-21 09:52:33
categories: Git
tags: [git本地提交]
---
## git的本地操作
首先先提出一个仓库的概念，git的仓库概念就是处处都是仓库，所以我可以轻易的clone本地的一些用git管理起来的文件夹（也就是书面语仓库），这里特别提一下服务器端的仓库，当然你可以像其他基本仓库一样，但是这样也就没有了服务器的优势（不可视，就比如linux服务器全部以无界面命令来进行操作，这样也就间接的避免了好多界面话导致的木马病毒的侵入），所以就有了裸仓库一说。服务器的git管理的最好是一个裸仓库。你也不需要在服务器端进行文件的一系列操作。万一误删了肿么办，死都不知道咋死的。。。
<!-- more -->
![两种仓库概念仓库](http://upload-images.jianshu.io/upload_images/1654122-be60690475a04540.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 在github上进行仓库的实战

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/1654122-ec43b579d93a59d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
README文件初学者建议不选。这个文件就是项目的说明文档，也就是你可以在里边写项目描述啥的，语法支持markdown如有需要请移步我的markdown文章，[MarkDown语法](http://www.jianshu.com/p/76a747786b04)

创建仓库之后你会看到你的仓库地址，就是https协议地址：
> https://github.com/*** 

还有ssh协议地址 git@github.com/***
不同的协议在你和仓库交互的时候会有不同的形式，前面我也说了，https你要输入github的用户名密码验证你自己的权限，ssh就是用之前生成和配置的rsa来验证，简化了输入账号密码的繁琐，而且安全了不止多少倍。
![不同协议下的仓库地址](http://upload-images.jianshu.io/upload_images/1654122-6a23bff0885955e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
好了，远程仓库 我们利用github这个网站创建好了，下面该提交咱们的代码到这个仓库了。
##git本地仓库创建、增加远程仓库依赖、提交、同步代码到远程和本地
这里咱们用一个很好玩的操作练习一下，这样大家都知道git远程仓库的本来面目了，你可以随便玩一玩，是不是感觉其实git也没什么高端的呢，哈哈

```
git clone **************.git --depth=1克隆深度1
```

![本地仓库模拟远程仓库](http://upload-images.jianshu.io/upload_images/1654122-b0c4a87b9cf3108d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
从上面图中可以看到cp命令(copy)，rm(remove)，cd(come into dir)
git add .将工作区中的所有文件跟踪进暂存区，git commit -m ""将暂存区的文件提交到仓库区，git status查看3大区的状态(绿色代表暂存区和仓库区没同步，红色代表工作区和暂存区没同步，像上边nothing to commit，working directory clean就是代表3大区全部同步完毕)只有全部同步完成后才能进行pull和push操作，git pull origin master将名字为origin的远程仓库中的名字为master的分支上的文件同步到本地工作区中，如果文件不存在冲突情况下，pull操作也会进行3大区的同步工作，特别情况下冲突了，无法进行同步，你就要自己同步了，先git status看一下哪个文件冲突了，修改后git add .和git commit -m ""工作区干净了之后就可以git push origin master这里我就不解释origin和master了，和pull一样，现在你也可以试一试了，不要怕错，git会永远保存你提交到仓库里的文件，每一次commit都会记录到git提交历史中，你可以进行回退到固定版本，查看历史提交的任何信息(记住是任何，所以不要怕丢文件，当然你只add不commit是不行的，这里你的电脑不挂就会永久保存，当然你不可能随时带着你的电脑了，可以将本地仓库文件push到远程仓库，除非放仓库文件的服务器仓库挂了，你的文件可以永久保存，并实时clone下来查看任何信息)
##本篇结语
> 流程通俗讲：文件---git管理版本-->保存在你电脑上--提交--->保>存到git的版本中，就是.git文件夹中，我就不带你们看了，怕你们迷->--push-->远程服务器文件，当然.git文件夹也是一起push过去的，>所以每次clone都是带一切版本历史纪录的

所以其实也就是一个被git管理每一次文件版本的文件在本地电脑和服务器电脑上互相同步数据。
语言组织好费脑细胞啊，请各位码友别介意哈^_^