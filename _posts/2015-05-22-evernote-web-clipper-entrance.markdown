---
layout: post
title: "英文版 Safari/Chrome 里剪藏如何登陆印象笔记"
date: 2015-05-22 17:23:21
category: 效率 技能
tags: Evernote  WebClipper
---


Evernote 应该称得上时下最为流行的云笔记了，在2012年进入中国市场，取名为印象笔记，现今在国内也是红红火火。

Evernote web clipper 在国内的名字叫剪藏，是一款浏览器扩展插件。几乎支持当前所以最为流行的浏览器，包括 Chrome、Safari、Firefox、IE、Opera。[下载地址：](https://evernote.com/webclipper/) ，很多英文版的用户在使用 Web clipper 的时候，会遇到这样一个问题：在登录的时候，无法找到印象笔记的登录入口，只有一个英文版的 Evernote international 登录入口。由于，国际版和国内版使用的是不同的数据库，用户使用印象笔记的账户登录国际版的 Evernote international 就会出现账户不存在这样的错误提示。那么，英文系统的用户如何顺利使用 Web clipper 登录印象笔记账户呢？

> 再看下面的内容之前，请看清楚这一个问题：你的电脑是使用英文版的浏览器（系统）吗？
> 如果不是，那你不需要往下面看了，因为你不会遇到这样的问题。
> 如果是，请继续往下面看。

网上看到很多办法是，强制修改浏览器的语言，我就拿 Chrome 来说吧，在 Mac 的 terminal 里面输入:

	defaults write com.google.Chrome AppleLanguages  '(zh_CN)'

重启 Chrome，浏览器变成中文的了，但是还是没有印象笔记的登录入口

**注意**，我的系统是英文版的。刚刚强制把 Chrome 换为中文的了。

所以，这个方法是不可行的。

那么，到底怎么才可以呢？下面这个方法，可以在不更换浏览器语言的情况下实现。

##Safari解决方法：

0.Safari 用户，先要开启 **develop** 模式。很简单，先进入 Safari 的 Preferences...（command＋","），找到 advance，勾选 show develop menu in menu bar，然后 Safari的 menu bar 上面多了一项 develop，当然，如果你的 Safari已经出现了 develop，这一步可以忽略。

1.点击 Safari toolbar 上面的剪藏图标，此时弹出你的登录界面是没有印象笔记登录入口的.

 2.这时，你就登录国际版 Evernote 账户，如果你没有，那就注册一个。总之，你需要先登录进去。为什么非要登录，进去才可以解决，在后面我会给我答案。

3.登录进去后，随便选择一个你需要剪藏的网页，点击剪藏图标就会出现这样的现象。

![pic3](http://7xj6ej.com1.z0.glb.clouddn.com/3.png)

4.再点击 options ，在页面中右键选择 inspect element（做前端开发的都知道这个吧），接着command＋F 查找`developerContainer`，我们看到这个 `div` 下面包含很多`div`（是一些开发者功能选项，就像很多程序一样，这些功能都被隐藏了，平时看不到），发现后面有一个 `style =“display:none;”`这是前端里面经常看到的一个样式语句，作用就是隐藏下面这个表里面的内容。

既然被隐藏了，那我们把它显示出来看看到底是什么东西？

双击 `display:none`会选中，把它们删掉。按 **enter** 键，此时奇迹就发神了。

![pic5](http://7xj6ej.com1.z0.glb.clouddn.com/5.png)

滚动页面，就会发现，多出了一个 developer options，这就是之前那些`<div>`里面的内容。好了，我们真正关心的只是其中一项叫：simulate simplified chinese 的复选框。选中它后面那个复选框，浏览器里面的内容发生变化了（结果很异常，不要害怕）
此时，淡定的 `command+Q` 退出 Safari 就可以了，然后再重启 Safari，点击 Web clipper 
看到了吗？里面多出了一个切换到印象笔记  的登录入口。自此，算是大功告成。

<br/>

##Chrome解决办法：

Chrome 相对来说，比 Safari 更简单，因为你不需要登录就可以实现。具体的办法是这样的。

1.右键 Chrome中的 web clipper，记住是右键，不是点击。

![pic8](http://7xj6ej.com1.z0.glb.clouddn.com/8.png)


2.点击 **options**，就看到这样的现象：

![pic9](http://7xj6ej.com1.z0.glb.clouddn.com/9.png)

怎么这个页面貌似很熟悉？

是的，这和之前 Safari 解决方法里面看到的页面一样，所以接下来的解决方法和 Safari 解决方法里面的3、4步一样。

重启 Chrome, Done!

<br/>
**SUMMARY**：

1.前面提到，为什么 Safari 必须要先登录，我们也发现 Chrome 不需要登录就可以实现。

因为 Web clipper 插件对 Chrome 提供了 option 选项，Safari 登录进去也是为了找到 option 选项，在 `html` 里面找到被隐藏的内容。

2.记住Mac中，强制转换程序语言：

	defaults write com.xxxx.appName AppleLanguages ‘(zh_CN)'
`zh_CN` 中文；`en_US` 美式英文.....

3.插件什么的都在浏览器里面呈现，它们都是是写成一个网页的，不是什么其他的语言（html、js、css）。因此，你会发现前端很强大。

4.为什么我只弄了 Chrome 和 Safari？

我想，其他浏览器和这二者的方法都是一样的，所以，其他的浏览器的小伙吧，都自己动手试试。而且我想Mac/win/Linux 也都是一样的吧。

Mac 上面，我个人的主力浏览器是 Safari，Chrome 是辅助浏览器。因为 Safari 对 Flash 支持不好，Chrome 不是主力是因为，Chrome 的优化不够好。Chrome 开启的情况下，Mac的温度直接逼近45度，甚至到50度；Safari 一般都维持在33-37度，续航能力强。

5.没有第5了。

<br/>
<p align="right">cap0dom / 2015-05-22</p>
