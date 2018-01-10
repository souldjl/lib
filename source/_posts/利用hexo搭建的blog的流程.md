---
title: 利用hexo搭建的blog的流程
date: 2017-08-08 11:43:43
tags: [笔记,教程]
copyright: true
---
# 按照流程进行如下初始环境配置
***
## 本地环境搭建

``` bash
$ npm install hexo-cli -g
```
``` bash
$ hexo init
```
``` bash
$ npm install
```
``` bash
$ hexo g
```
``` bash
$ hexo s
```
<table><tr><td bgcolor=orange>运行localhost:4000端口即可访问</td></tr></table>
<!--more-->
***

## hexo基本命令：
1. hexo g #完整命令为hexo generate，用于生成静态文件
2. hexo s #完整命令为hexo server，用于启动服务器，主要用来本地预览
3. hexo d #完整命令为hexo deploy，用于将本地文件发布到github上
4. hexo n #完整命令为hexo new，用于新建一篇文章
5. hexo clean 清除缓存

***
## 更改默认主题配置 
下载next (配置了大众化的next)
``` bash
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
``` 
修改根目录配置文件
theme: next



***
# 发布文章
每次发布执行以下3步即可
1.  hexo clean 
2.  hexo generate
3.  hexo deploy
***

# 新建文章
hexo new "xxx"
在source目录下找到新建页面修改即可。
***

#删除文章
eg:(helloworld)
&nbsp;&nbsp;&nbsp;&nbsp;先删除本地文件，然后通过生成和部署命令进而将远程仓库中的文件也一并删除。
&nbsp;&nbsp;&nbsp;&nbsp;具体来说，以最开始默认形成的helloworld.md这篇文章为例。首先进入到source / _post 文件夹中，
&nbsp;&nbsp;&nbsp;&nbsp;找到helloworld.md文件，在本地直接执行删除。然后依次执行hexo g，hexo d，这就是如何删除文章的方法。


***
# 同步远程仓库

## 修改配置
修改根目录配置文件_config.yml
&nbsp;&nbsp;&nbsp;&nbsp;     type: git
&nbsp;&nbsp;&nbsp;&nbsp;   repository: https://github.com/username/username.github.io.git
&nbsp;&nbsp;&nbsp;&nbsp;   branch: master
## hexo deploy 发布
<font color=#f00 size=4>* ERROR: &nbsp;&nbsp;&nbsp;Deployer not found : github</font>
&nbsp;&nbsp;&nbsp;&nbsp; <font color=orange >npm install hexo-deployer-git --save</font>
      








