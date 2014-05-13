---
layout: post
category: android-development
title: NineOldAndroids动画库
description: ""
modified: 2014-05-13
tags: [android]
comments: true
share: true
image:
  feature: abstract-2.jpg
  credit: 
  creditlink: 
comments: true
---


##介绍

![](https://raw.githubusercontent.com/baoyongzhang/test_pages/gh-pages/NineOldAndroids.png)

Android3.0推出了全新的[AnimationAPI](http://android-developers.blogspot.com/2011/02/animation-in-honeycomb.html)，使用起来很方便，但是不能在3.0以下版本使用，[NineOldAndroids](https://github.com/JakeWharton/NineOldAndroids)是一个可以在任意Android版本上使用的AnimationAPI，API和Android3.0中的类似。

###常用类
* ObjectAnimator
* ValueAnimator 
* AnimatorSet 
* ViewPropertyAnimator 

类名与官方的API是对应的，只是包名为`com.nineoldandroids.animation`。


![](https://raw.githubusercontent.com/baoyongzhang/test_pages/gh-pages/NineOldAndroid_demo.gif)

##使用方法
首先导入NineOldAndroids的jar包。在Android3.0中，`View`中有一个`animate`方法，NineOldAndroids中提供了`ViewPropertyAnimator.animate(View)`与其对应，可以选择静态导入。

{% highlight java %}

// 官方API（3.0以上）
mView.animate().setDuration(5000).rotationY(720).x(100).y(100).start();
		
// NineOldAndroids
ViewPropertyAnimator.animate(mView).setDuration(5000)
				.rotationY(720).x(100).y(100).start();
				
// 可以使用静态导入
import static com.nineoldandroids.view.ViewPropertyAnimator.animate;
// 直接调用animate方法
animate(mView).setDuration(5000).rotationY(720).x(100).y(100).start();

{% endhighlight %}

使用链式编程设置各种属性参数，最终调用`start()`来启动动画，还可以调用`setStartDelay()`设置动画延迟启动。

可以设置动画的监听器，在动画开始、结束等时候加一些处理。


{% highlight java %}


ViewPropertyAnimator
	.animate(mIView)
	.setDuration(5000)
	.rotationY(720)
	.x(100).y(100)
	.setListener(new AnimatorListenerAdapter() {
		@Override
		public void onAnimationStart(Animator animation) {
			super.onAnimationStart(animation);
			// 动画开始
		}
		@Override
		public void onAnimationCancel(Animator animation) {
			super.onAnimationCancel(animation);
			// 动画取消
		}
		@Override
		public void onAnimationEnd(Animator animation) {
			super.onAnimationEnd(animation);
			// 动画结束
		}
		@Override
		public void onAnimationRepeat(Animator animation) {
			super.onAnimationRepeat(animation);
			// 动画重复启动
		}
	}).start();


{% endhighlight %}

`ViewPropertyAnimator`对象提供了取消动画的方法

{% highlight java %}
ViewPropertyAnimator animate = ViewPropertyAnimator.animate(mDropTv);
/* ... */
animate.start();	// 开始动画
animate.cancel();	// 取消动画

{% endhighlight %}

简单的动画效果使用`ViewPropertyAnimator`一般可以满足，下面介绍一下高级玩法。核心是`ObjectAnimator`类。

###举例

* 背景颜色从红色到蓝色，并反转回去，而且无限重复。

{% highlight java %}


ValueAnimator colorAnim = ObjectAnimator.ofInt(mView, "backgroundColor", /*红色*/0xFFFF8080, /*蓝色*/0xFF8080FF);
colorAnim.setDuration(3000);
colorAnim.setEvaluator(new ArgbEvaluator());	// ARGB
colorAnim.setRepeatCount(ValueAnimator.INFINITE);   // 无限重复
colorAnim.setRepeatMode(ValueAnimator.REVERSE); // 反转回去
colorAnim.start();


{% endhighlight %}

* 使用动画集合`AnimatorSet`，可以使用多个`ObjectAnimator`，实现复杂的动画效果。

{% highlight java %}


AnimatorSet set = new AnimatorSet();
set.playTogether(
    ObjectAnimator.ofFloat(myView, "rotationX", 0, 360),
    ObjectAnimator.ofFloat(myView, "rotationY", 0, 180),
    ObjectAnimator.ofFloat(myView, "rotation", 0, -90),
    ObjectAnimator.ofFloat(myView, "translationX", 0, 90),
    ObjectAnimator.ofFloat(myView, "translationY", 0, 90),
    ObjectAnimator.ofFloat(myView, "scaleX", 1, 1.5f),
    ObjectAnimator.ofFloat(myView, "scaleY", 1, 0.5f),
    ObjectAnimator.ofFloat(myView, "alpha", 1, 0.25f, 1)
);
set.setDuration(5 * 1000).start();


{% endhighlight %}

###ObjectAnimator说明

ObjectAnimator是动画对象，通过ObjectAnimator提供的一系列of开头的静态方法创建。



使用`ObjectAnimator`需要传入`PropertyName`，`PropertyName`可以参考`ViewPropertyAnimator.animate()`中提供的方法名。

##总结
NineOldAndroids的API与官方的API基本一致，使用很方便。能够轻松实现各种酷炫动画效果。
一般情况使用

##参考
* Github主页：https://github.com/JakeWharton/NineOldAndroids
* 官方网站：http://nineoldandroids.com/
* ListView动画库，使用了NineOldAndroids：https://github.com/nhaarman/ListViewAnimations
* 官方API：http://android-developers.blogspot.com/2011/02/animation-in-honeycomb.html
