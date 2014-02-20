---
layout: post
category: android-development
title: REST设计风格的理解及在Android开发中的应用
description: "REST设计风格的理解及在Android开发中的应用"
modified: 2014-02-11
tags: [actionbar]
comments: true
share: true

comments: true
---
#一、简介

##1.     参考资料:

[维基百科_REST](http://zh.wikipedia.org/wiki/REST)<br/>
[维基百科_讨论:REST](http://zh.wikipedia.org/wiki/Talk:REST)<br/>
[IBM developerWorks_最佳实践：更好的设计你的 REST API](https://www.ibm.com/developerworks/cn/web/1103_chenyan_restapi/)

##2.     主要特征

* 资源是由URI来指定。
* 对资源的操作包括获取、创建、修改和删除资源，正好对应HTTP协议提供的GET、POST、PUT和DELETE方法。
* 通过操作资源的表现形式来操作资源。
* 资源的表现形式则是XML、HTML或任何其他格式，取决于使用场景。


##3.     要求

* 客户端和服务器结构
* 连接协议具有无状态性
* 能够利用Cache机制增进性能
* 层次化的系统
* 随需代码

#二、理解

##1、关于：资源是由URI来指定

       以互邀网（www.whoyao.com）为例，在场馆部分有一些由运维人员展示的图片，图片的链接可能指向站内不同的模块、也可能指向站外。
       在开发互邀网APP时，站内链接要求应用内页面跳转，站外链接使用浏览器跳转。这个时候互邀网的REST模式发挥了作用，Android客户端使用UriMatcher.addURI()、UriMatcher.match()和Uri.getPathSegments()方法对URL进行解析，获取所需参数完成跳转。
       

##2、关于：连接协议具有无状态性

       互邀网WebApi早期开发的接口未实现“无状态行”。以计算用户与某活动地点距离的接口为例 getActivityDistance(int activityID)。计算距离所必须的经纬度，是由另外一个接口上传的，不符合“无状态性”。这在使用时必须两次请求才能获取最新的距离。
	   
       后期开发的获取与场馆距离的方法，直接要求传入经纬度，getVenueDistance(double longitude, double latitude, int venueID)，虽然单次请求的参数增加了，但不再需要两次请求。
       
#三、扩展知识

##1、Android中的REST

       Android中的ContentProvider是最典型的REST设计方式，所有的数据操作均采用URI方式。
	   
       因此Android中提供了很好用的URI生成、解析工具，给使用REST时间方式带来了很大遍历。（相关方法可参加Android资料中ContentProvider相关部分）
       
##2、常见误区之HTTP方法

       有一个常见的误区，就是REST的查、增、改、删操作应对应HTTP的GET、POST、PUT、DELETE方法。其实采用这种对应的方式只是可选，维基百科_讨论：REST建议单独使用GET或POST方式。
	   
       事实上，采用4种操作方式是违反了“资源是由URI来指定”的，在Android的ContentProvider中就不存在这么多操作方式，对数据的操作都是在URI中标记的。
       
##3、WebApi设计中的REST

@ //TODO 未完待续
 