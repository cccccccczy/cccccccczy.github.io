---
layout: post
title:  "Android 之 hello world"
date:   2016-11-22 17:04:15
categories: Android
excerpt: Android hello world
---

* content
{:toc}

### 引言

刚开始接触Android对于我这种零基础甚至不会Java的渣渣当然是一脸懵逼，于是在各种课程网站上找视频、查资料，从Hello World开始一点点的学。虽然自己平时已经很忙，但还是决定每过一段时间就要有一些进步。写博客和笔记还真的是一个反思自己是否有所进步的好办法呢。

就像Dev C++和VC的对比一样，总觉得eclipse界面看起来简洁好操作，颜值很符合个人口味。但是查了一些资料和按照实验室的要求发现，Android Studio正在被大力推广，也将成为Android开发使用工具的潮流，所以我决定从这里开始入门。

实践是最好的学习方法，嘻嘻。

### 环境搭建

- 下载Android SDK （翻墙）

  ![](http://upload-images.jianshu.io/upload_images/323464-4ce26de1799f4aaf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 安装SDK

  注意要提前安装好JDK且配置好环境变量，否则安装时会找不到JDK。

- 启动

  首次启动会询问一些设置，不过之后都可以改。

  ![](http://upload-images.jianshu.io/upload_images/323464-887736cd3a050e0b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  ​下载相关组件。

![](http://upload-images.jianshu.io/upload_images/323464-4ef0fa87c1fd3bd6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  ​	到这里之后就差不多都弄好了，不过我的电脑在后面运行时发现一些问题，需要在BIOS界面设置一些东西，后面再说。

  ​	顺便一提，个人觉得深色的界面看起来比较高大上，就改了。具体为：File-Settings->在Editor里面选colors&fonts，把默认的改成Darcula（黑色）就好啦~
  <br>

### 新建项目

  1. 点击 Start a new Android Studio project

  2. 设置项目名、域名（包名）和项目路径。

  3. 设置SDK的最低兼容版本，不知道怎么选就默认了。

  4. 选择初始Activity，Blank就行。

  5. 设置MainActivity的相关属性，默认即可。

  6. 自动构建项目。 任何代码都没有写，不知道AS是不是默认了生成一个helloworld。

  7. 选择一个模拟器来运行程序，我选了默认的。

     嗯......这时候问题来了，出现了一个“Intel VT-X not enabled”的东西，上网查发现是BIOS里面的“Virtualization Technology”选项没有打开，因此无法使用HAXM。重启了好几次才进去BIOS，改一下设置就好啦~

     ![IMG_20141222_143735](http://www.2cto.com/uploadfile/Collfiles/20150303/2015030310320831.jpg)

  8. 运行

     ​点上面的绿色的小三角，just wait

     ​看到手机界面上出现hello world的时候，内心万分激动！！！！！嘻嘻

     ​![](http://ww3.sinaimg.cn/large/006pzljrgw1fa10e9vdj8j309t0egt9e.jpg)

<br>

​	嘻嘻~![](http://ww1.sinaimg.cn/large/006pzljrgw1fa0z73jk5tj301c01c0sh.jpg)
