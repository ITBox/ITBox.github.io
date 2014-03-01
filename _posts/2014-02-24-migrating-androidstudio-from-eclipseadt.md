---
layout: post
category: android-development
title: 从EclipseADT迁移到AndroidStudio
description: ""
modified: 2014-02-24
tags: [android]
comments: true
share: true
image:
  feature: abstract-2.jpg
  credit: 
  creditlink: 
comments: true
---
[AndroidStudio](http://developer.android.com/sdk/installing/studio.html#Revisions)是Google I/O 2013大会上推出的Android开发环境，基于[IntelliJ IDEA](http://www.jetbrains.com/idea/)，和EclipseADT类似。[AndroidStudio](http://developer.android.com/sdk/installing/studio.html#Revisions)目前尚处于测试版，还是存在一些Bug的，当然取代EclipseADT只是时间问题，现在大部分应该都还是在使用EclipseADT开发，但是有必要适应一下这个新工具。AudioStudio使用[Gradle](http://www.gradle.org/)来构建项目，什么是[Gradle](http://www.gradle.org/)呢？[Gradle](http://www.gradle.org/)是一个自动化编译部署测试工具，类似于[Ant](http://ant.apache.org/)、[Maven](http://maven.apache.org/)。

* [AndroidStudio官方主页](http://developer.android.com/sdk/installing/studio.html)
* [为何 IntelliJ IDEA 比 Eclipse 更好](http://www.oschina.net/news/26929/why-intellij-is-better-than-eclipse)
* [Gradle等构建系统使用介绍](http://www.ibm.com/developerworks/cn/opensource/os-cn-gradle/)

下载安装过程不多说，本文主要介绍如果将EclipseADT的项目迁移到AndroidStudio。

####1. 为项目生成Gradle所需的文件
* 我们先看看EclipseADT中项目的节本结构

![](https://github.com/baoyongzhang/test_pages/blob/gh-pages/image-4.png?raw=true)

* 接下来为项目生成Gradle所需的相关文件，这里注意一下，ADT版本必须是22以上的，不是的在SDK Manager中更新到最新版本。接下来，在项目上右键->Export，然后选择Generate Gradle build files。
![](https://github.com/baoyongzhang/test_pages/blob/gh-pages/image-3.png?raw=true)
![](https://github.com/baoyongzhang/test_pages/blob/gh-pages/image-5.png?raw=true)

* 一路next，直到finish。搞定，这时候已经生成的Gradle的相关文件，看下一现在的结构。
![](https://github.com/baoyongzhang/test_pages/blob/gh-pages/image-6.png?raw=true)

####2. 从AudioStudio中导入项目
* 打开AudioStudio，选择Import Project，选择项目的目录。OK即可导入。
![](https://github.com/baoyongzhang/test_pages/blob/gh-pages/image-7.png?raw=true)

* 主要就是build.gradle文件。
![](https://github.com/baoyongzhang/test_pages/blob/gh-pages/image-8.png?raw=true)

#### 扩展阅读
* 参考官方说明 [Migrating from Eclipse](http://developer.android.com/sdk/installing/migrate.html)
* [AndroidStudio快捷键](http://www.android-studio.org/index.php/docs/experience/142-androidstudio-shortcut-keys)


<iframe src="http://baidu.com" width="500" height="200"></iframe>

* ----------

<object width="450" height="500" align="middle" id="reader" codebase="http://fpdownload.macromedia.com/pub/shockwave/cabs/flash/swflash.cab#version=6,0,0,0" classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000">
<param value="window" name="wmode">
<param value="true" name="allowfullscreen"><param name="allowscriptaccess" value="always">
<param value="http://wenku.baidu.com/static/flash/apireader.swf?docurl=http://wenku.baidu.com/play&amp;docid=a04a62ea360cba1aa811da7f&amp;title=%E5%80%92%E9%9C%89%E7%86%8A%E7%BB%8F%E5%85%B8ppt&amp;doctype=ppt&amp;fpn=5&amp;npn=5&amp;readertype=external&catal=0&amp;cdnurl=http://txt.wenku.baidu.com/play" name="movie"><embed width="250" align="middle" height="200" pluginspage="http://www.macromedia.com/go/getflashplayer" type="application/x-shockwave-flash" name="reader" src="http://wenku.baidu.com/static/flash/apireader.swf?docurl=http://wenku.baidu.com/play&amp;docid=a04a62ea360cba1aa811da7f&amp;title=%E5%80%92%E9%9C%89%E7%86%8A%E7%BB%8F%E5%85%B8ppt&amp;doctype=ppt&amp;fpn=5&amp;npn=5&amp;readertype=external&catal=0&amp;cdnurl=http://txt.wenku.baidu.com/play" wmode="window" allowscriptaccess="always" bgcolor="#FFFFFF" ver="9.0.0" allowfullscreen="true">
</embed>
</object>
