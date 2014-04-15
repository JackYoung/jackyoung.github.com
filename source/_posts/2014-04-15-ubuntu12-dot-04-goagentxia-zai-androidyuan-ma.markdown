---
layout: post
title: "Ubuntu12.04 GoAgent下载Android 4.0源码"
date: 2014-04-15 21:37:38 +0800
comments: true
categories: Android
---
由于GFW的原因在国内下载Android源码经常因为网络错误导致无法下载，故而试着通过Goagent来进行下载。先按照教程下载配置 [GoAgent][Goagent link]。    

###设置代理
设置curl和repo的代理，如果有自定义GoAgent的代理端口，则需要把8087替换为自定义的代理端口。
```sh
export http_proxy = http://127.0.0.1:8087
export HTTP_PROXY = http://127.0.0.1:8087
export HTTPS_PROXY = https://127.0.0.1:8087
```
###下载repo
1、在当前用户目录下创建bin目录，并把该目录添加到终端的环境变量中。
```sh
$ mkdir ~/bin
$ PATH=~/bin:$PATH
```
2、下载repo并设置权限
```sh
$ curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > ~/bin/repo
$ chmod a+x ~/bin/repo
```
###初始化repo
1、创建一个Android源码的路径，并进入。
```sh
$ mkdir ~/android-4.0.1_r1
$ cd android-4.0.1_r1
```
2、指定repo要下载的分支，此时会提示你需要输入邮箱和姓名，按照提示输入即可。
```sh
$ repo init -u https://android.googlesource.com/platform/manifest -b android-4.0.1_r1
```
###下载源码
```sh
$ repo sync
```

[Goagent link]:https://code.google.com/p/goagent/wiki/GoAgent_Linux