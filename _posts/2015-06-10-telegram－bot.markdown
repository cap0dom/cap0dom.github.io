---
title: "Telegram-bot开发入门教程"
date: 2015-06-10
category: lua
tags: telegram lua

layout: post
---

##前言

`telegram` 这一款来自战斗名族的IM产品，从目前的情况来看确实扛起了俄罗斯人在移动社交领域的大旗，正浩浩荡荡向全世界宣战。在 Google 里面随便搜索 telegram 都会出现很多砖家|报道分析...不知它今后会怎样，但是目前来看，确实称得上一款很不错的通讯工具。更多具体的描述可以点 [这里](https://www.telegram.org)。

<br/>

##Telegram Bot 

当我第一次接触到这个机器人的时候，觉得很惊艳。我当时的情景是:有朋友输入 `!img ***` 然后就随机得到了一个关于 *** 的图片。一怒之下，就开始 Google 寻找相应的实现方法。最后在 Github 上面找到了。

在 github 里面检索 `telegram bot` 出来的结果中，最靠前的是 telegram-bot 和 tg 。这也是实现整个机器人的方法。如果你具备一点点 `git` 基础，就接着往下看。如果，你还不知道 `git` 那么请转[廖雪峰老师的博客](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000) 。

<br/>

##安装telegram-cli环境

[Telegram-cli](https://github.com/vysheng/tg) 是Telegram Command Line Interface的缩写，是托管在 github 上面的一个项目。telegram-bot是基于telegram-cli的。在开始前，申明一下，目前telegram-cli 只工作在liunx 和 OS X，对于windows 用户，如果你想尝试，要么虚拟机，要么可以尝试用你的android手机安装环境。
这里主要说明OS X 环境下面的 telegram-cli 环境安装。

首先,你要保证你的 OS X 具备[Homebrew](http://brew.sh/) 或者 [Macports](https://www.macports.org)，如果，没有安装这些管理工具，先行安装。这里主要基于 `Homebrew`
在终端里面执行下面的命令，一行一行执行。

     brew install libconfig
     brew install readline
     brew install lua
     brew install python
     brew install libevent
     export CFLAGS="-I/usr/local/include -I/usr/local/Cellar/readline/6.3.8/include"
     export LDFLAGS="-L/usr/local/lib -L/usr/local/Cellar/readline/6.3.8/lib"
     ./configure && make
	
如果，你遇到:
- python "...gcc compiler no"这样的安装失败错误，请到 [stackoverflow](http://stackoverflow.com/)，查找错误，解决问题。提示一下：一半是你的xcode developTool 的问题。

然后，编译。同样在终端里面

    env CC=clang CFLAGS=-I/usr/local/include LDFLAGS=-L/usr/local/lib LUA=/usr/local/bin/lua52 LUA_INCLUDE=-I/usr/local/include/lua52 LUA_LIB=-llua-5.2 ./configure
     make

接下来，clone 源代码

    git clone https://github.com/yagop/telegram-bot.git && cd telegram-bot

然后开始安装
    ./launch.sh install

需要花一点时间，等待安装完成。

最后运行

    ./launch.sh

在运行的时候，会需要你输入你的手机号码，就是你注册 telegram 时的电话号码(是国际格式，也就是你的电话号码前要添加+86)，然后，你会收到
一个密码 via SMS && Telegram。填入就完成了。
有些可能还会遇到，需要你输入密码，那就输入你的 telegram注册时的密码，最后成功启动telegram-bot。

<br/>

##测试

机器人正常启动了，那怎么测试呢？

很简单，你在群里面或者和任意人聊天，只要有人输入支持plugin 对应的命令，你都会自动触发信息。此时，你的ID和机器人ID是同一个ID，也就是说，你具备机器人属性了。比如：有人发送`!help all` 你会自动发送plugins list。
当然，你可以为机器人单独注册一个ID，或者你自己再重新注册一个号，此时你本身就与机器人独立了。
说直白一点，机器人于用户是一摸一样的，只是，它从不发信息，直到有支持的 plugin命令触发，所以，你想让机器人独立出来，你需要注册两个telegram 账号。

**tips**: 要想机器人一直正常工作，必须保证./launch.sh 一直运行。

<br/>

##深入

前面说的，都是安装好telegram-bot就直接使用了，并没有做适合个人特殊需求的开发。所以，如果，你想实现更多好玩的功能，就需要动手开发更多的plugin。比如：你可以实现，用手机给机器人发一条`!shutdown now`命令，让电脑关机。具体的实现方法，留着今后有机会再说。


<p style="text-align:right">cap0dom / 2015-05-23</p>

<br/>
