---
title: 乱搞旧手机Xshell+Termux防坑记
author: hyp-rge
avatar: /images/avatar.png
comments: false
mathjax: true
date: 2019-06-11 16:50:19
authorLink:
authorAbout:
authorDesc:
categories: OI - 其他
tags:
keywords:
description:
photos: https://i.loli.net/2019/06/11/5cff6c69cb68d59487.png
---
没有动力写代码了就搞点别的,就有了这一篇文章(x
关于SSH,如果不是\lq老师讲课时曾经用过Xshell我都不会想到这么玩(
先放张成果图,我觉得整体体验是海星的:
字体:`PragmataPro`
{% fb_img https://i.loli.net/2019/06/11/5cff6c7bcde1c91799.png %}

Termux是啥就不多说了，前置芝士和配置参考:
[Termux高级终端安装使用配置教程 - FreeBuf互联网安全新媒体平台](https://www.freebuf.com/geek/170510.html)
[PC端利用Xshell连接Android上的Termux - 路安达 - 博客园](https://www.cnblogs.com/Luad/p/10191667.html)
剩下的内容网上都是互相抄...开始自行摸索

记录一些踩的坑:
- vim里按下backspace键回到行首
这锅得Xshell背...
属性 - 终端 - 键盘 `DELETE键序列`和`BACKSPACE键序列`都选第二项`ASCII 127`
似乎Windows上的gvim也会有这个问题,当时搞了半天~~弄不好就弃了~~
- 装了zsh脚本后出现的storage文件夹里啥都没有
{% fb_img https://i.loli.net/2019/06/11/5cff6e942416e76760.png %}
我怎么感觉这种方式能访问SD卡是以讹传讹...该怎么搞还怎么搞嘛,如图
{% fb_img https://i.loli.net/2019/06/11/5cff6f07c8a1888749.png %}
- `$PREFIX/include/bits/` 里没有stdc++.h
~~对OI玩家极不友好~~ 手动复制一份进去
- 怎么传文件?
这玩意儿没有`rz`命令qwq ~~(`sz`倒是有,喷了)~~ 不想次次都插线就用SFTP吧
- Hello World
{% fb_img https://s2.ax1x.com/2019/06/11/VgJOTP.png %}
{% fb_img https://s2.ax1x.com/2019/06/11/VgJLwt.png %}
这个warning我也不知道怎么肥四啊 ~~反正看起来没什么影响~~
